## 基础操作
```sh
# 查看本地全部镜像
docker images
# 拉取镜像
docker pull 镜像名称[:tag]
# 如: docker pull daocloud.io/library/tomcat:8.5.15-jre8
# 删除本地镜像
docker rmi 镜像的标识
# 将本地的镜像导出
docker save -o 导出的路径 镜像id
#加载本地的镜像文件
docker load -i 镜像文件
#修改镜像文件
docker tag 镜像id 新镜像名称：版本
```

## 自定义镜像

- Dockerfile
创建自定义镜像就需要创建一个Dockerfiler,如下为Dockerfile的语言：

```
from：指定当前自定义镜像依赖的环境
copy：将相对路径下的内容复制到自定义镜像中
workdir：声明镜像的默认工作目录
run：执行的命令，可以编写多个
cmd：需要执行的命令（在workdir下执行的，cmd可以写多个，只以最后一个为准）
```
示例：
```
from daocloud.io/library/tomcat:8.5.15-jre8
copy ssm.war /usr/local/tomcat/webapps
```

- 通过Dockerfile制作镜像
编写完Dockerfile后需要通过命令将其制作为镜像，并且要在Dockerfile的当前目录下，之后即可在镜像中查看到指定的镜像信息，注意最后的 .

```sh
docker build -t 镜像名称[:tag] ./
```
