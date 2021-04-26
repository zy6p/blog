---
title: 开源 GIS
date: 2021-04-26 11:13:03
tags: opengis gis qgis
---
# OpenGIS

## 开源 GIS 简介

开源 GIS 软件允许我们查看源码，自由使用，对学习 GIS 底层逻辑、编程技术大有裨益。  

### 为什么选择开源 GIS

专用的 GIS 软件（ ArcGIS ），可以让我们轻松完成所有 GIS 工作。但是，该软件通常非常昂贵，并不能查看源码，不能从中学到编码技能。  

GIS 供应商有时会为教育活动提供例外，他们提供了更便宜或免费或容易破解的软件。他们这样做的目的是，如果老师和学习者了解他们的软件，他们将不愿意学习其他软件包。当学习者离开学校时，他们将进入工作场所并购买商业软件，而永远不会知道他们可以使用免费的替代品。  

### GIS 教育现状

**ArcGIS 几乎等于 GIS**

* 大二有一门课是地理信息系统导论，课程所用软件就是 ArcGIS
* 大三的空间分析也是用的 ArcGIS
* 大三下的地理信息系统实践还是用的 ArcGIS

### 关于 ArcGIS

合理怀疑 ArcGIS 破解这么容易就是 ESRI 公司故意的。目的就是让大家熟悉 ArcGIS 产品，然后卖 ArcGIS Pro 。无论百度、谷歌还是淘宝，几乎没有可用的 Pro 的破解版。  

## 主流开源 GIS 软件

### 桌面 GIS

* QGIS: 桌面 GIS 软件大哥，能实现可视化地理数据、空间分析等功能，界面美观易用，支持插件扩展，提供 python api 以供开发。
* GRASS: 拥有众多空间分析功能，在 OSGEO 组织推动下绝大部分功能集成进 QGIS 了。
* SAGA: 空间分析软件，也被集成到 QGIS 了。

### GIS 库（C++）

* GDAL: 抽象地理数据库，GIS 的底层核心，影响巨大。
* PROJ: 投影库，被 GDAL 使用。
* GEOS: 二维矢量库，被 PostGIS 使用，提供有许多算法。

### GIS 服务器

* GeoServer: 主流 GIS 服务器，支持 WMS WFS WCS WTMS 等。

### WebGIS 客户端

* OpenLaysers: 功能全面的 JavaScript 库，支持多种投影，支持多种 OGC 服务。
* Leaflet: 轻量级 JS 库，可以使用插件扩展。
* Cesium: 三维 JS 库，确立了三维规范。

### 更多

更多 GIS 相关开源项目可以关注如下资源:

* [OSGEO](https://www.osgeo.org/): 开源 GIS 组织。
* [awesome-gis](https://github.com/sshuair/awesome-gis): 有趣的 GIS 项目集。
* [OGC](https://www.ogc.org/): 制定了众多 GIS 规范。

## 基础知识

### GIS 基础概念

#### 传统

* 几何: 点、线、面、集合
* 数据: 矢量、栅格、Tin
* 格式: Shp、GeoJSON、GeoPackage、GeoTIFF
* 地理: 经纬度、坐标系、投影
* 样式: sld、kml

#### 三维

* 数据: 点云、glTF
* 图形: OpenGL、WebGL、DirectX

#### WebGIS

* 协议: WMS、WTMS、WFS、WCS、WPS、XYZ

#### 数据科学

* 空间分析: 数据格式、算法
* 大数据: Hadoop、Kafka

#### 图形

* 图形学: 物理渲染、Shader、图形硬件
* 渲染引擎: Unity、UE4

