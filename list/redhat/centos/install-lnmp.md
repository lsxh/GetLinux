# 安张LNMP环境

> 作为一个PHP程序员，需要懂得在Linux下安装环境，这是基本的技能！现在让我们手动的来安装吧，我们将使用三种方式来安装我们的环境，即通过在线安装，编译安装以及脚本安装，来进行我们的操作！你准备好了吗？

**在线安装：** 在线软件仓库的使用，给我们带来许多便利，这种模式使得我们不再需要将时间花费在漫长的编译等待上，而是直接使用已经编译好的rpm包来安装，省时省力，便于维护和升级，但是可制定程度不高。

**编译安装：** 将耗费大量的时间在编译上，并且随时可能出现各种错误，但是你可以自定义包的安装和配置文件的位置，在一定程度上，编译安装性能要高。可制定程度高，但是维护比较麻烦。

**脚本安装：** 脚本安裝一般是通过sh执行文件来安装的一种便捷安装方式，使用这种方式，不再需要自己手动一个个软件从头安装到尾，而是使用交互界面，来完成安装选择，方便快捷


下面我们在CentOS7下安装 Nginx + PHP + MairaDB

## 安装Nginx

### 1. 使用yum在线安装

```bash
sudo yum install nginx -y
```

### 2. 使用编译安装方式

1. 首先先安装依赖和库

```bash
sudo yum install gcc gcc-c++ pcre pcre-devel zlib zlib-devel openssl openssl-devel
```

2. 获取nginx源文件包(当前的稳定版本)

```bash
wget -c https://nginx.org/download/nginx-1.12.2.tar.gz
```

3. 解压文件

```bash
tar -zvxf nginx-1.12.2.tar.gz
```

4. 进入目录

```bash
cd nginx-1.12.2
```

5. 使用默认配置,将nginx安装到/usr/local/nginx下

```bash
./configure --prefix=/usr/local/nginx
```

6. 编译安装

```bash
 make && sudo make install
 ```

7. 添加链接到/usr/bin 下，便于快速启动

```bash
sudo ln -s /usr/local/nginx/sbin/nginx /usr/bin/nigix
```

8. nginx的进程管理

```bash
    sudo nginx                  #启动
    sudo nginx -s stop      #强制停止
    sudo nginx -s quit       #等待nginx任务处理完毕后停止
    sudo nginx -s reload    #重载配置文件
    ##查看进程是否启动
    ps aux|grep nginx
```

9. 如需开机启动，尝试下列方法

> 编辑此文件,末尾添加nginx的路径地址

```bash
vim /etc/rc/local       #添加如下内容到文件末位置

/usr/local/nginx/sbin/nginx
```

10. 设置执行权限

```bash
sudo chmod 755 /etc/rc.local
```

## 安装PHP

### 使用yum源安装php

```bash
sudo yum install php
```

### 使用yum安装，安装前，需要安装部分扩展和依赖

```bash
sudo yum install php-mysql php-gd php-ldap php-odbc php-pear php-xml php-xmlrpc php-mbstring php-bcmath php-mhash
```

### 通过编译安装PHP

### 安装php7需要的一些依赖库包 libxml2和一些其他依赖的扩展库

```bash
sudo yum install libxml2 libxml2-devel openssl openssl-devel curl-devel libjpeg-devel libpng-devel freetype-devel bzip2-devel libmcrypt libmcrypt-devel postgresql-devel aspell-devel readline-devel libxslt-devel mysql-devel sqlite-devel gmp-devel db4-devel openldap openldap-devel enchant-devel libvpx-devel libXpm-devel libc-client-devel libicu-devel unixODBC-devel net-snmp-devel
```

### 安装前的环境配置检查，php7的一些依赖包的检查和php扩展的启动，这个过程如果缺少php依赖的库包会有报错提示。

### 添加用户和组

```bash
sudo groupadd -r www
sudo useradd -r -g www -s /sbin/nologin
```

### 开始配置

```bash
./configure --prefix=/usr/local/php --with-config-file-path=/usr/local/php/etc --enable-fpm --with-fpm-user=www --with-fpm-group=www --enable-inline-optimization --disable-debug --disable-rpath --enable-shared --enable-soap --with-xmlrpc --with-openssl --with-mcrypt --with-pcre-regex --with-sqlite3 --with-zlib --enable-bcmath --with-iconv --with-bz2 --enable-calendar --with-curl --with-cdb --enable-dom --enable-exif --enable-fileinfo --enable-filter  --with-pcre-dir --enable-ftp --with-gd --with-openssl-dir --with-jpeg-dir --with-png-dir --with-freetype-dir --enable-gd-native-ttf --enable-gd-jis-conv --with-gettext --with-gmp --with-mhash --enable-json --enable-mbstring --enable-mbregex --enable-mbregex-backtrack --with-libmbfl --with-onig --enable-pdo --with-mysqli=mysqlnd --with-pdo-mysql=mysqlnd --with-zlib-dir --with-pdo-sqlite --with-readline --enable-session --enable-shmop --enable-simplexml --enable-sockets --enable-sysvmsg --enable-sysvsem --enable-sysvshm --enable-wddx --with-libxml-dir --with-xsl --enable-zip --enable-mysqlnd-compression-support --with-pear --enable-opcache
```

### 编译 安装

```bash
make && sudo make install
```

## 安装MariaDB

> 由于MySQL存在闭源的风险，现在redhat系列的发行版本已经将默认的MySQL数据库更换为MariaDB，在此处，我们以MariaDB为例，展示安装配置

### 使用yum命令安装

> 由于CentOS仓库的包版本可能相对较低，因此，我们手动添加我们的yum源。

1. 添加 MariaDB yum 仓库
```bash
vi /etc/yum.repos.d/MariaDB.repo
```

2. 插入如下内容
```bash
[mariadb]
name = MariaDB
baseurl = http://yum.mariadb.org/10.1/centos7-amd64
gpgkey=https://yum.mariadb.org/RPM-GPG-KEY-MariaDB
gpgcheck=1
```

3. 安装 MariaDB
```bash
sudo yum install MariaDB-server MariaDB-client -y
```

4. MariaDB的进程管理
```bash
systemctl start mariadb         #启动服务
systemctl enable mariadb    #开机启动
systemctl status mariadb    #状态查看
systemctl stop mariadb      #停止服务
```

5. 对 MariaDB 进行安全配置
> 通过这项命令，设置 MariaDB 的 root 账户密码，禁用 root 远程登录，删除测试数据库以及测试帐号，最后需要使用下面的命令重新加载权限。使用前，请确保你已经启动了MariaDB服务，否则将报错。
```bash
mysql_secure_installation
```

6. 尝试登陆数据库(使用你自己的密码)
```bash
mysql -u root -p
```

### 通过编译安装MariaDB

> 待续。。。

> 本文有借鉴：[PHP自学网](http://zixuephp.net/article-207.html)，[Linux就该这样学](http://www.linuxprobe.com/centos7-mariadb10-easy.html)