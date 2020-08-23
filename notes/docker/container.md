## 运行容器
运行容器需要定制具体镜像，如果镜像不存在，会直接下载
```sh
docker run 镜像的标识|镜像的名称[:tag]
# 常用的参数
docker run -d -p 宿主机端口:容器端口 --name 容器名称 镜像的标识|镜像名称[:tag]
# -d:代表后台运行容器
# -p 宿主机端口:容器端口：为了映射当前Linux的端口和容器的端口
# --name 容器名称:指定容器的名称
```

## 查看正在运行的容器

```sh
# 查看全部正在运行的容器信息
docker ps [-qa]
# -a 查看全部的容器，包括没有运行
# -q 只查看容器的标识
```

## 查看容器日志
```sh
docker logs -f 容器id
# -f：可以滚动查看日志的最后几行
```

## 进入容器的内部
```sh
docker exec -it 容器id bash
```
## 复制内容到容器

```sh
# 将宿主机的文件复制到容器内部的指定目录
docker cp 文件名称 容器id:容器内部路径
```

## 重启&启动&停止&删除容器
```sh
# 重新启动容器
docker restart 容器id

# 启动停止运行的容器
docker start 容器id

# 停止指定的容器(删除容器前，需要先停止容器)
docker stop 容器id

# 停止全部容器
docker stop $(docker ps -qa)

# 删除指定容器
docker rm 容器id

# 删除全部容器
docker rm $(docker ps -qa)
```