---
title: ZOO-Project WPS 服务框架基础
date: 2021-05-01 23:18:11
tags:
 - gis
 - opengis
 - wps
---
# ZOO-Project

ZOO-Project 是一个 WPS（Web Processing Service）框架，实现了 OGC 的 WPS 1.0.0 和 WPS 2.0.0 标准。  

ZOO-Project 是一个用C、Python 和 JavaScript 编写的WPS（网络处理服务）实现。它是一个开源的平台，实现了由开放地理空间协会（OGC）编辑的WPS 1.0.0和WPS 2.0.0标准。  

ZOO-Project 为创建和连锁符合WPS的网络服务提供了一个开发者友好的框架。它的主要目标是提供通用的和符合标准的方法来使用现有的开源库和算法作为WPS。它还为创建新的创新网络服务和应用提供了有效的工具。  

ZOO-Project 能够在线处理地理空间或非地理空间数据。它的核心处理引擎（又称ZOO-Kernel）让你在可靠的软件和库的基础上执行一些现有的ZOO-服务。它还使您能够从新的或现有的源代码中创建自己的WPS服务，这些代码可以用七种不同的编程语言编写。这让您可以简单地将代码编成或变成WPS服务，具有直接的配置和标准编码方法。  

ZOO-Project 在数据输入和输出方面非常灵活，因此您可以处理几乎任何一种存储在本地或从远程服务器和数据库访问的数据。ZOO-Project在数据处理和整合新的或现有的空间数据基础设施方面表现出色，因为它能够与地图服务器通信，并能整合网络绘图客户端。  

## 特点

1. 模块化。分为4个模块，分别是 ZOO-Kernel，ZOO-Services，ZOO-API，ZOO-Client。
2. 扩展性。开发人员可以使用任意熟悉的语言开发相关算法，无需像 GeoServer 只能使用 JAVA。

## 安装

### docker 安装

使用作者发布在阿里云上的 docker 镜像部署.

```shell
git clone git@github.com:zy6p/zoo-project.git
docker-compose up -d
```

接下来可以访问 [http://localhost:8166/](http://localhost:8166/)

