---
title: Mac OS Docker连接本地数据库
date: 2020-05-12 14:54:55
tags:
  - Docker
categories:
  - Frontend
---


###### Docker启动的服务在Macos上从容器内部连接容器外的数据库

```bash
# Ctr + R
    docker run -d -p 8080:3000 --name wiki --restart unless-stopped -e "DB_TYPE=postgres" -e "DB_HOST=docker.for.mac.host.internal" -e "DB_PORT=5432" -e "DB_USER=postgres" -e "DB_PASS=postgres" -e "DB_NAME=wiki" requarks/wiki:2.2.51

```

可以看出，如果想要从容器内部访问本地数据库，需要使用`docker.for.mac.host.internal`来访问，使用172.0.0.1或IP都无法连接到（原因可能是Docker是以虚拟机的形式在macos上启动）.
