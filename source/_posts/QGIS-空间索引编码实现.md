---
title: QGIS 空间索引编码实现
date: 2021-05-14 12:25:58
tags:
---
# QGIS 空间索引编码实现

QGIS 空间索引使用 R 树索引，主要底层实现由库 [libspatialindex](https://github.com/libspatialindex/libspatialindex) 提供。

## 构建空间索引

QGIS 原生算法包含“创建空间索引”算法，其参数仅包含一项 -- 输入图层。该算法由 C++ 实现，编码如下：  

### 调用 QgsVectorDataProvider 的 createSpatialIndex() 函数

```c++
QVariantMap QgsSpatialIndexAlgorithm::processAlgorithm( const QVariantMap &parameters, QgsProcessingContext &context, QgsProcessingFeedback *feedback )
{
  QgsVectorLayer *layer = parameterAsVectorLayer( parameters, QStringLiteral( "INPUT" ), context );

  if ( !layer )
    throw QgsProcessingException( QObject::tr( "Could not load source layer for %1." ).arg( QLatin1String( "INPUT" ) ) );

  QgsVectorDataProvider *provider = layer->dataProvider();

  if ( provider->capabilities() & QgsVectorDataProvider::CreateSpatialIndex )
  {
    if ( !provider->createSpatialIndex() )
    {
      feedback->pushInfo( QObject::tr( "Could not create spatial index" ) );
    }
  }
  else
  {
    feedback->pushInfo( QObject::tr( "Layer's data provider does not support spatial indexes" ) );
  }

  QVariantMap outputs;
  outputs.insert( QStringLiteral( "OUTPUT" ), layer->id() );
  return outputs;
}
```

### QgsVectorDataProvider 的 createSpatialIndex() 为虚函数，由其派生类实现

#### bool QgsOgrProvider::createSpatialIndex()

  * QgsOgrProvider 仅支持 shp gpkg sqlite 构建空间索引，对其他数据格式不支持。
  * 是线程不安全的，查询和更新必须从互斥中运行。

```c++
bool QgsOgrProvider::createSpatialIndex()
{
  QgsCPLHTTPFetchOverrider oCPLHTTPFetcher( mAuthCfg );
  QgsSetCPLHTTPFetchOverriderInitiatorClass( oCPLHTTPFetcher, QStringLiteral( "QgsOgrProvider" ) );

  if ( !mOgrOrigLayer )
    return false;
  if ( !doInitialActionsForEdition() )
    return false;

  QByteArray layerName = mOgrOrigLayer->name();
  if ( mGDALDriverName == QLatin1String( "ESRI Shapefile" ) )
  {
    QByteArray sql = QByteArray( "CREATE SPATIAL INDEX ON " ) + quotedIdentifier( layerName );  // quote the layer name so spaces are handled
    QgsDebugMsgLevel( QStringLiteral( "SQL: %1" ).arg( QString::fromUtf8( sql ) ), 2 );
    mOgrOrigLayer->ExecuteSQLNoReturn( sql );

    if ( !mFilePath.endsWith( QLatin1String( ".shp" ), Qt::CaseInsensitive ) )
      return true;

    QFileInfo fi( mFilePath );     // to get the base name
    //find out, if the .qix file is there
    return QFileInfo::exists( fi.path().append( '/' ).append( fi.completeBaseName() ).append( ".qix" ) );
  }
  else if ( mGDALDriverName == QLatin1String( "GPKG" ) ||
            mGDALDriverName == QLatin1String( "SQLite" ) )
  {
#if QT_VERSION < QT_VERSION_CHECK(5, 14, 0)
    QMutex *mutex = nullptr;
#else
    QRecursiveMutex *mutex = nullptr;
#endif
    OGRLayerH layer = mOgrOrigLayer->getHandleAndMutex( mutex );
    QByteArray sql = QByteArray( "SELECT CreateSpatialIndex(" + quotedIdentifier( layerName ) + ","
                                 + quotedIdentifier( OGR_L_GetGeometryColumn( layer ) ) + ") " ); // quote the layer name so spaces are handled
    mOgrOrigLayer->ExecuteSQLNoReturn( sql );
    return true;
  }
  return false;
}
```

#### bool QgsMemoryProvider::createSpatialIndex()

  * 基于内存的图层
  * 此处调用库 libspatialindex 实现

```c++
bool QgsMemoryProvider::createSpatialIndex()
{
  if ( !mSpatialIndex )
  {
    mSpatialIndex = new QgsSpatialIndex();

    // add existing features to index
    for ( QgsFeatureMap::iterator it = mFeatures.begin(); it != mFeatures.end(); ++it )
    {
      mSpatialIndex->addFeature( *it );
    }
  }
  return true;
}
```

```c++
void QgsSpatialIndexData::initTree( IDataStream *inputStream = nullptr )
{
  // for now only memory manager
  mStorage = StorageManager::createNewMemoryStorageManager();

  // R-Tree parameters
  double fillFactor = 0.7;
  unsigned long indexCapacity = 10;
  unsigned long leafCapacity = 10;
  unsigned long dimension = 2;
  RTree::RTreeVariant variant = RTree::RV_RSTAR;

  // create R-tree
  SpatialIndex::id_type indexId;

  if ( inputStream && inputStream->hasNext() )
    mRTree = RTree::createAndBulkLoadNewRTree( RTree::BLM_STR, *inputStream, *mStorage, fillFactor, indexCapacity,
             leafCapacity, dimension, variant, indexId );
  else
    mRTree = RTree::createNewRTree( *mStorage, fillFactor, indexCapacity,
                                    leafCapacity, dimension, variant, indexId );
}
```
