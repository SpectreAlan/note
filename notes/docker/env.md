## Centos

```sh
# 卸载旧版
sudo yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine
# 安装yum-utils
sudo yum install -y yum-utils
  device-mapper-persistent-data \
  lvm2
# 安装依赖（可选）
sudo yum install -y device-mapper-persistent-data lvm2
# 指定Docker镜像源
yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
# 安装docker
sudo yum install docker-ce docker-ce-cli containerd.io
# 启动docker服务
systemctl start docker
# 设置开机自动启动
systemctl enable docker
# 测试
docker run hello-world
```
