---
title: OSGeo 国内镜像源
date: 2021-05-04 21:57:45
tags:
 - gis
 - opengis
---
# OSGeo 国内镜像源

## Windows

OSGeo4w 镜像：[武汉大学地理加权建模实验室](http://gwmodel.whu.edu.cn/mirrors/osgeo4w/)，镜像手动同步，有时会造成软件出现 Bug。

## Ubuntu

ubuntugis 反向代理：[科大镜像](https://mirrors.ustc.edu.cn/)

```shell
sudo sed -i 's/ppa.launchpad.net/mirrors.ustc.edu.cn/g' /etc/apt/sources.list/*
```

## Docker

### PostGIS

GitHub 仓库: [zy6p/docker-postgis](https://github.com/zy6p/docker-postgis)

```shell
docker pull registry.cn-hangzhou.aliyuncs.com/opengis/postgis
```

### Zoo

```shell
git clone git@github.com:zy6p/zoo-project.git
docker-compose up -d
```

