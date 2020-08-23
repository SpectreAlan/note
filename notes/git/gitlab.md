## gitlab安装
```sh
# 安装依赖
sudo yum install -y policycoreutils-python openssh-server
# ssh开机自启
sudo systemctl enable sshd
# ssh启动
sudo systemctl start sshd
# postfix安装
sudo yum install -y postfix
# postfix开机自启
sudo systemctl enable postfix
# 修改postfix配置
vim /etc/postfix/main.cf
# 找到 inet_interfaces = localhost，将其改为inet_interfaces = all，:wq保存退出，
# postfix启动
sudo systemctl start postfix
# 添加gitlab包仓库
curl https://packages.gitlab.com/install/repositories/gitlab/gitlab-ce/script.rpm.sh | sudo bash
# 安装gitlab
sudo EXTERNAL_URL="你Gitlab服务器运行所在的公网IP地址" yum install -y gitlab-ce
# 检查gitlab运行状态
gitlab-ctl status
```

## gitlab常用命令
```sh
# 查看状态：
gitlab-ctl status
# 查看日志：
gitlab-ctl tail
```

##  其他
- 配置文件：
/etc/gitlab/gitlab.rb
- 安装目录：
/opt/gitlab
- 日志目录：
/var/log/gitlab
gitlab包安装自带的服务：nginx/redis/postgres/ruby on rails/unicorn