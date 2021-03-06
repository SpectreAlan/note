> 概念：将宿主机的一个目录映射到容器的一个目录中

> 效果：可以在宿主机中操作目录中的内容，那么容器内部映射的文件，也会跟着一起改变

## 创建数据卷
创建数据卷后，默认会存放在一个目录下/var/lib/docker/volumes/数据卷名称/_data
```sh
docker volume create 数据卷名称
```
## 查看全部数据卷
```sh
docker volume ls
```
## 查看数据卷详情
查看数据卷的详细信息，可以查询到存放的路径，创建时间等等
```sh
docker volume inspect 数据卷名称
```
## 删除数据卷
```sh
docker volume rm 数据卷名称
```
## 容器映射数据卷
- 通过数据卷名称映射
如果数据卷不存在。Docker会帮你自动创建，会将容器内部自带的文件，存储在默认的存放路径中。

```sh
docker run -d -p 8080:8080 --name tomcat -v 数据卷名称:容器内部的路径 镜像id
 ```

- 通过路径映射数据卷
直接指定一个路径作为数据卷的存放位置。但是这个路径下是空的。

```sh
docker run -d -p 8080:8080 --name tomcat -v 路径(/root/自己创建的文件夹):容器内部的路径 镜像id
```
