## 下载Docker-Compose
github搜索并下载docker-compose，以1.24.1版本为例

> https://github.com/docker/compose/releases/download/1.24.1/docker-compose-Linux-x86_64

## 设置权限
```sh
mv docker-compose-Linux-x86_64 docker-compose
chmod 777 docker-compose
```
## 配置环境变量
将docker-compose文件移动到了/usr/local/bin，修改了/etc/profile文件，给/usr/local/bin配置到了PATH中
```sh
mv docker-compose /usr/local/bin
vi /etc/profile
#添加内容：export PATH=$JAVA_HOME:/usr/local/bin:$PATH
source /etc/profile
```

## 测试
在任意目录下输入docker-compose

## docker-compose.yml
> yml文件以key:value方式来指定配置信息

> 多个配置信息以换行+缩进的方式来区分

> 在docker-compose.yml文件中，不要使用制表符

```yml
version: '3.1'
services:
  mysql:           # 服务的名称
    restart: always   # 代表只要docker启动，那么这个容器就跟着一起启动
    image: daocloud.io/library/mysql:5.7.4  # 指定镜像路径
    container_name: mysql  # 指定容器名称
    ports:
      - 3306:3306   #  指定端口号的映射
    environment:
      MYSQL_ROOT_PASSWORD: root   # 指定MySQL的ROOT用户登录密码
      TZ: Asia/Shanghai        # 指定时区
    volumes:
     - /opt/docker_mysql_tomcat/mysql_data:/var/lib/mysql   # 映射数据卷
  tomcat:
    restart: always
    image: daocloud.io/library/tomcat:8.5.15-jre8
    container_name: tomcat
    ports:
      - 8080:8080
    environment:
      TZ: Asia/Shanghai
    volumes:
      - /opt/docker_mysql_tomcat/tomcat_webapps:/usr/local/tomcat/webapps
      - /opt/docker_mysql_tomcat/tomcat_logs:/usr/local/tomcat/logs
```
## 容器管理
在使用docker-compose的命令时，默认会在当前目录下找docker-compose.yml文件
```sh
# 启动
docker-compose up -d
 
# 关闭并删除容器
docker-compose down
 
# 开启|关闭|重启已经存在的由docker-compose维护的容器
docker-compose start|stop|restart
 
# 查看由docker-compose管理的容器
docker-compose ps
 
# 查看日志
docker-compose logs -f
```

## docker-compose管理自定义镜像
### 编写docker-compose文件
```yml
version: '3.1'
services:
  ssm:
    restart: always
    build:            # 构建自定义镜像
      context: ../      # 指定dockerfile文件的所在路径
      dockerfile: Dockerfile   # 指定Dockerfile文件名称
    image: ssm:1.0.1
    container_name: ssm
    ports:
      - 8081:8080
    environment:
      TZ: Asia/Shanghai
```
### 编写Dockerfile文件
```
from daocloud.io/library/tomcat:8.5.15-jre8
copy ssm.war /usr/local/tomcat/webapps
```
### 运行
如果自定义镜像不存在，会帮助我们构建出自定义镜像，如果自定义镜像已经存在，会直接运行这个自定义镜像
```sh
docker-compose up -d
# 重新构建自定义镜像
docker-compose build
# 运行当前内容，并重新构建
docker-compose up -d --build
```