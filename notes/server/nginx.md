## 常规安装

```
# 安装wget
yum install -y weget
# 安装编译工具和相关的库文件 
yum -y install make zlib zlib-devel gcc-c++ libtool  openssl openssl-devel
# 下载PCRE（让nginx支持RUL地址的重定向功能）
wget https://sourceforge.net/projects/pcre/files/pcre/8.41/pcre-8.41.tar.gz
# 解压
tar -xvzf pcre-8.41.tar.gz
# 进入安装包目录
cd pcre-8.41
# 编译安装
./configure
make && make install
# 下载安装 nginx
wget http://nginx.org/download/nginx-1.14.0.tar.gz
# 解压
tar -xvzf nginx-1.14.0.tar.gz
# 进入安装包目录：
cd nginx-1.14.0
# 编译安装：
./configure --prefix=/usr/local/share/applications/nginx-1.14.0/ --with-http_ssl_module
make && make install
# 查找nginx安装路径
find / -name nginx
# 制作nginx软链接,格式为：ln -s 目标路径 /usr/sbin/nginx
# 找到带sbin的nginx路径，如：/usr/local/share/applications/nginx-1.14.0/sbin/nginx
ln -s /usr/local/share/applications/nginx-1.14.0/sbin/nginx /usr/sbin/nginx
```

## yum安装

### 添加yum源

root权限下添加nginx的yum源，此处以RHEL/CentOS为例，其他版本的linux参考[这里](http://nginx.org/en/linux_packages.html)

```
# 安装yum-utils
yum install yum-utils -y
# 添加nginx.repo
vi /etc/yum.repos.d/nginx.repo
```

内容如下：

```
[nginx-stable]
name=nginx stable repo
baseurl=http://nginx.org/packages/centos/$releasever/$basearch/
gpgcheck=1
enabled=1
gpgkey=https://nginx.org/keys/nginx_signing.key
module_hotfixes=true

[nginx-mainline]
name=nginx mainline repo
baseurl=http://nginx.org/packages/mainline/centos/$releasever/$basearch/
gpgcheck=1
enabled=0
gpgkey=https://nginx.org/keys/nginx_signing.key
module_hotfixes=true
```

### 安装nginx

```bash
yum -y install nginx
```

## 启动nginx

```
nginx
# 如果没任何提示，则代表nginx启动成功了，查看nginx端口占用
lsof -i:80
# nginx   1410  root    6u  IPv4  22898      0t0  TCP *:http (LISTEN)
# nginx   2206 nginx    6u  IPv4  22898      0t0  TCP *:http (LISTEN)
# 开放80端口
firewall-cmd --permanent --zone=public --add-port=80/tcp
# 重载防火墙
firewall-cmd --reload
```

## 修改nginx配置

```
# 查找配置文件路径
find / -name nginx.conf
# 得到路径如：/etc/nginx/nginx.conf，接下来编辑
vi /etc/nginx/nginx.conf
```

找到server部分

```
server {
    listen       80; #监听的端口
    server_name  aaa.com; # 需要绑定的域名   
    location / {
        root   /www/xxx; # 项目路径
        index  index.html index.htm; # 默认首页
    }
    # 反向代理
    location /api/ { # 将所有带api的请求代理到127.0.0.1:3000
        proxy_pass   http://127.0.0.1:3000;    # host:port的格式
        proxy_set_header X-Real-IP $remote_addr; # 代理以后获取真实访问源ip
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for; # 在多层代理时会包含真实客户端及中间每个代理服务器的IP
        proxy_set_header Host $http_host; # 客户端真实的域名和端口号
        proxy_set_header X-Forwarded-Proto $scheme; # 客户端真实的协议
        proxy_set_header X-NginX-Proxy true;
        
    }
}
server {
    listen       80;
    server_name  bbb.com; # 需要绑定的域名   
    location / {
        root   /www/yyy; # 项目路径
        index  index.html index.htm; # 默认首页
    }
    # 反向代理
    location /admin/ { # 将所有带admin的请求代理到127.0.0.1:4000
        proxy_set_header Host   $host:$proxy_port;
        proxy_set_header X-Real-IP  $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_pass   http://127.0.0.1:4000;    # host:port的格式
    }
}
```

配置完毕以后esc然后:wq回车，reload一下conf

```bash
nginx -s reload 
```

## nginx常规操作

```
# 启动nginx
nginx

# 重新载入配置文件
nginx -s reload 

# 重启 nginx
nginx -s reopen   
        
# 停止 nginx
nginx -s stop

# 添加开机启动
systemctl enable nginx.service

# 查看nginx错误日志(最近100条)
tail -100f /var/log/nginx/error.log

# 查看nginx请求日志(最近100条)
tail -100f /var/log/nginx/access.log
```
## 错误处理
### 报错403

- 查看nginx的启动用户
-ps aux | grep "nginx: worker process"   如果提示nobody，则将nginx.conf配置文件的user改为root或正确的用户
- 项目根目录缺少index.html等默认首页
- SELinux设置为开启状态（enabled）的原因
解决： /etc/selinux/config下将SELINUX=enforcing 修改为 SELINUX=disabled 状态
- 权限问题，如果nginx没有web目录的操作权限
解决：开放权限chmod -R 777 /www/xxx/或者chown为nginx

### 报错413
这个错误一般就是上传接口的文件大小超过限制了
解决： 在nginx.conf的server内添加一行：
client_max_body_size 2m;
ps：2m根据实际需要调整
