## 安装MySQL

- 常规安装

```sh
# 查看是否有自带的MySql库，如果先有卸载
rpm -qa | grep mysql
# 删除mysql-lib(系统自带的版本过低)
yum remove mysql-libs
# 下载mysql
wget https://dev.mysql.com/get/Downloads/MySQL-5.7/mysql-5.7.12-1.el6.x86_64.rpm-bundle.tar 
# 解压
tar -xvf mysql-5.7.12-1.el6.x86_64.rpm-bundle.tar  
# 依次安装mysql包（common、libs、client、server）
rpm -ivh mysql-community-common-5.7.12-1.el6.x86_64.rpm
rpm -ivh mysql-community-libs-5.7.12-1.el6.x86_64.rpm
rpm -ivh mysql-community-client-5.7.12-1.el6.x86_64.rpm  
rpm -ivh mysql-community-server-5.7.12-1.el6.x86_64.rpm 
```

- yum安装

```sh
# 下载mysql的yum源
wget https://dev.mysql.com/get/mysql57-community-release-el7-9.noarch.rpm
# 安装rpm
rpm -ivh mysql57-community-release-el7-9.noarch.rpm
# 安装mysql
cd /etc/yum.repos.d/
yum install mysql-server -y
```

## mysql常规操作

```sh
# 启动
systemctl start mysqld 
# 查看默认密码
grep 'password' /var/log/mysqld.log
# [Note] A temporary password is generated for root@localhost: x2sX3Gb6+Dtm
# root@localhost: 这里后面就是默认密码
# 登陆
mysql -uroot -p
# 修改默认密码
SET PASSWORD = PASSWORD('Abcd1234.');
# ps：这里需要大小写数组字符相结合，不然会通不过
# 刷新系统权限
flush privileges;
# 开启远程登录权限
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY 'Abcd1234.' WITH GRANT OPTION;
# 刷新系统权限
flush privileges;
# 接下来就可以远程登陆了
```