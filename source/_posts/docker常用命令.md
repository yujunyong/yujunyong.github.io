title: docker常用命令
date: 2015-12-15 16:52:02
categories: docker
tags: docker
---

# 进入容器
## 进入没有运行的容器
``` sh
docker run -i -t container /bin/bash
```

## 进入正在运行的容器
如果使用的是1.3.0版本以上的Docker,使用以下命令:
``` sh
docker exec -it container bash
```

如果是使用更老的版本，使用以下命令：
``` sh
docker run --rm --volume=/usr/local/bin:/target jpetazzo/nsenter
sudo docker-enter gitlab
```

# 删除镜像/容器
## 删除所有的容器
``` sh
docker rm $(docker ps -a -q)
```

## 删除所有的镜像
``` sh
docker rmi $(docker images -q)
```
