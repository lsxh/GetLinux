# 安张LNMP环境

> 作为一个PHP程序员，需要懂得在Linux下安装环境，这是基本的技能！现在让我们手动的来安装吧，我们将使用三种方式来安装我们的环境，即通过在线安装，编译安装以及脚本安装，来进行我们的操作！你准备好了吗？

**在线安装：** 在线软件仓库的使用，给我们带来许多便利，这种模式使得我们不再需要将时间花费在漫长的编译等待上，而是直接使用已经编译好的rpm包来安装，省时省力，便于维护和升级，但是可制定程度不高。适合小环境，核心功能都具备，快速搭建环境。

**二进制包安装：** 它是发布出来时预先编译过的，既避免了编译的麻烦，又提供了增强功能。

**编译安装：** 将耗费大量的时间在编译上，并且随时可能出现各种错误，但是你可以自定义包的安装和配置文件的位置，在一定程度上，编译安装性能要高。可制定程度高，但是维护比较麻烦。它的要求高，要有编译环境，编译时可指定几乎所有选项，可满足你的所有选择。

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

1. 使用yum安装，需要安装的部分扩展和依赖

```bash
sudo yum install php-mysql php-gd php-ldap php-odbc php-pear php-xml php-xmlrpc php-mbstring php-bcmath php-mhash
```

### 通过编译安装PHP

1. 安装php7需要的一些依赖库包 libxml2和一些其他依赖的扩展库

```bash
sudo yum install libxml2 libxml2-devel openssl openssl-devel curl-devel libjpeg-devel libpng-devel freetype-devel bzip2-devel libmcrypt libmcrypt-devel postgresql-devel aspell-devel readline-devel libxslt-devel mysql-devel sqlite-devel gmp-devel db4-devel openldap openldap-devel enchant-devel libvpx-devel libXpm-devel libc-client-devel libicu-devel unixODBC-devel net-snmp-devel
```

> 安装前的环境配置检查，php7的一些依赖包的检查和php扩展的启动，这个过程如果缺少php依赖的库包会有报错提示。在你的机器上可能由于仓库源的问题，某些包安装会报错，建议安装epel源：sudo yum install epel-release

2. 添加用户和组

```bash
sudo groupadd -r www
sudo useradd -r -g www -s /sbin/nologin www
```

3. 开始配置

```bash
./configure --prefix=/usr/local/php --with-config-file-path=/usr/local/php/etc --enable-fpm --with-fpm-user=www --with-fpm-group=www --enable-inline-optimization --disable-debug --disable-rpath --enable-shared --enable-soap --with-xmlrpc --with-openssl --with-mcrypt --with-pcre-regex --with-sqlite3 --with-zlib --enable-bcmath --with-iconv --with-bz2 --enable-calendar --with-curl --with-cdb --enable-dom --enable-exif --enable-fileinfo --enable-filter  --with-pcre-dir --enable-ftp --with-gd --with-openssl-dir --with-jpeg-dir --with-png-dir --with-freetype-dir --enable-gd-native-ttf --enable-gd-jis-conv --with-gettext --with-gmp --with-mhash --enable-json --enable-mbstring --enable-mbregex --enable-mbregex-backtrack --with-libmbfl --with-onig --enable-pdo --with-mysqli=mysqlnd --with-pdo-mysql=mysqlnd --with-zlib-dir --with-pdo-sqlite --with-readline --enable-session --enable-shmop --enable-simplexml --enable-sockets --enable-sysvmsg --enable-sysvsem --enable-sysvshm --enable-wddx --with-libxml-dir --with-xsl --enable-zip --enable-mysqlnd-compression-support --with-pear --enable-opcache
```

4. 编译 安装

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

### 通过二进制分发包安装MariaDB

> 通过二进制包的形式，可以快速解决软件版本问题，又不需要浪费太长的时间在编译上，我们只需要解压文件后通过简单的配置，就可以使用他们。

**MariaDB安装注意：** 安装路径必须指定在/usr/local目录下，并且目录名称必须命名为mysql，否则某些脚本将会无法运行，导致无法安装

1. 下载二进制包。下载地址:https://downloads.mariadb.org

    我选择的是MariaDB 10.0 Stable 版本，现在已经到了33的小版本了，大家按需选择版本

    我的机器是64位的，对应 OS/CPU 选项栏 Linux x86_64 可以看到三个包提供选择，区别不是特别大，我们选择普适的包 mariadb-10.0.33-lnux-x86_64.tar.gz。

1. 解压文件

    在下载完成后，将其上传到Linux机器上，使用如下命令解压

        tar -zvxf mariadb-10.0.33-lnux-x86_64.tar.gz

    解压后得到同名的文件夹 mariadb-10.0.33-lnux-x86_64，可以尝试着自行查看安装操作步骤，该文件是:INSTALL-BINARY

1. 创建用户

    我们要创建一个mysql的用户，它可以对以后的mysql数据库进行管理，同时我们还可以指定mysql的家目录，这样以后它的存储数据就可以独立出来放置了，同时指明shell类型为nologin

        groupadd mysql
        useradd -g mysql -s /sbin/nologin mysql

1. 文件夹移动

    将解压出来的文件夹移动到/usr/local文件夹下，并重命名或者创建软链接为mysql

        mv  mariadb-10.0.33-lnux-x86_64 /usr/local/mysql    #直接重命名
        或者:mv  mariadb-10.0.33-lnux-x86_64 /usr/local/      #先拷贝到文件夹 后创建软连接，此方法便于后期版本管理
        cd /usr/local
        ln -s mariadb-10.0.33-lnux-x86_64 mysql

1. 确保你使用了正确的my.cnf文件

    my.cnf为MariaDB的配置文件，在mariadb安装目录下的support-files有好几种配置模板，已经配置好的部分参数，分别用于不同的环境。

    1. my-small.cnf 这个是为小型数据库或者个人测试使用的，不能用于生产环境

    1. my-medium.cnf 这个适用于中等规模的数据库，比如个人项目或者小型企业项目中，

    1. my-large.cnf 一般用于专门提供SQL服务的服务器中，即专门运行数据库服务的主机，配置要求要更高一些，适用于生产环境

    1. my-huge.cnf 用于企业级服务器中的数据库服务，一般更多用于生产环境使用

    我们暂且使用my-small.cnf文件

        mv /etc/my.cnf /etc/my.cnf.bak      #备份默认配置
        cp support-files/my-huge.cnf  /etc/mysql/my.cnf     #使用新的配置文件

1. 编辑配置文件

    在[mysqld]块中添加basedir全局目录将默认的数据目录，日志目录，pid文件都放置在basedir目录下

    ```bash
    vim  /etc/my.cnf
    basedir = /usr/local/mysql      #此处添加你的实际路径到[mysqld]模块下
    ```

1. 初始化

        cd /usr/local/mysql/
        ./scripts/mysql_install_db --user=mysql


1. 权限设置

        chown -R mysql .
        chgrp -R mysql data

1. 启动脚本

    ```bash
    bin/mysqld_safe --user=mysql &
    ```

    **注意：** 出现错误提示mysql是对/var/log/这个目录没有写权限，请手动创建文件夹并给予权限

    添加mysql到系统服务目录

    ```bash
    cp support-files/mysql.server /etc/init.d/mysqld
    ```

    第一次安装需要手动启动服务

    ```bash
    /etc/init.d/mysqld start
    ```

    添加mysqld到系统服务，随系统一起启动

    ```bash
     chkconfig mysqld on
    ```

    查看mysql服务运行状态：

    ```bash
    systemctl status mysqld.service
    ```

1. 添加环境变量

    ```bash
    vi /etc/profile
    export  PATH=/usr/local/mysql/bin:$PATH
    source /etc/profile
    ```

1. 尝试登陆

    ```bash
    mysql_secure_installation       #数据库安全配置，在此可以设置root密码
    mysql -u yourname -p yourpassword       #登陆命令
    ```

> 本文有借鉴：[PHP自学网](http://zixuephp.net/article-207.html)，[Linux就该这样学](http://www.linuxprobe.com/centos7-mariadb10-easy.html)