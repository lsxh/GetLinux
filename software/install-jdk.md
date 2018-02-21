# 安装配置jdk

> 在跨平台操作下，java开发环境的部署显得很重要，此篇记载java开发环境的部署

<br />

**JDK下载官网**:[点击这里](http://www.oracle.com/technetwork/java/javase/downloads/index.html)下载对应平台的jdk版本

## 1. 在linux下的二进制包安装方法(以8u144版本为例)

> 下载已经编译好的jdk二进制源码包：**jdk-8u144-linux-x64.tar.gz**，按照习惯，解压到 **/usr/local/** 目录下，解压后文件路径是：**/usr/local/jdk1.8.0_144**。

```bash
#解压命令：
tar -zxvf jdk-8u144-linux-x64.tar.gz /usr/local
```

### 设置环境变量：

```bash
vi /etc/profile    #所有用户有效
vi ~/.bash_profiel    #对当前用户有效
#上面两个命令任选其一，我选择对所有用户有效 在文件末尾添加如下内容：

JAVA_HOME=/usr/local/jdk1.8.0_144

JRE_HOME=/usr/local/jdk1.8.0_144/jre

CLASS_PATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar:$JRE_HOME/lib

PATH=$PATH:$JAVA_HOME/bin:$JRE_HOME/bin

export JAVA_HOME JRE_HOME CLASS_PATH PATH

#保存文件  :wq
#文件生效
source /etc/profile

#验证是否生效
java -version
#若成功显示版本信息，则无需继续下面操作

#若系统自带有openjdk,需要修改默认jdk版本，这几步比较重要
sudo update-alternatives --install /usr/bin/java java /usr/local/jdk1.8.0_144/bin/java 8114
#上面的参数只需要修改/usr/local/jdk1.8.0_144/bin/java为自己的目录即可，8114是一个标示，没什么太大的意义，后面的一步一样的原理
sudo update-alternatives --install /usr/bin/javac javac /usr/local/jdk1.8.0_144/bin/javac 8114

#设置默认的java版本(输入编号回车即可)：
sudo update-alternatives --config java
sudo update-alternatives --config javac

#再来看我们的java版本，是不是变了呢！！
java -version

#对于更新一个 /usr/bin/xxx 命令指向一个新的路径可以用 sudo update-alternatives --config xxx
```

#### 好啦！！大功告成！！下面开始快捷的jdk安装

## 2. 在redhat系列的机器上

有两个快捷的方式来进行软件的安装。其一是通过rpm软件包管理命令来进行安装 **.rpm** 结尾的软件，在JDK下载官网下载以.rpm为后缀的文件即可。我们下载jdk-8u144-linux-x64.rpm这个，来到下载目录进行包的安装，命令如下：

```bash
#来到下载文件夹下，我的是家目录的download文件夹下
cd ~/download
#安装
rpm -ivh jdk-8u144-linux-x64.rpm
#软件默认安装在/usr/java里面，然后如上面一样，配置环境变量
```

第二种方式要方便的多，使用yum命令。先来查看一下yum仓库都有一些什么jdk版本

```bash
yum search java | grep jdk
```

可以发现仓库中只有openjdk可以安装，如果你认为有必要，你可以选择版本进行安装，命令如下：

```bash
yum install java-1.8.0-openjdk
```

**使用yum安装，环境变量默认已经配置好了。再开始在Debian上进行快捷安装**

## 3. 在Debian机器上

Debian系列的机器可以使用apt-get来进行软件的快速安装，先来看一下apt仓库上有哪些jdk版本

   ```bash
   apt-cache search java|grep jdk
   ```

   发现仍然是以openjdk为主，有需要，选择安装

   ```bash
   sudo apt-get install openjdk-8-jdk
   ```

这里说一下openjdk和Oracle jdk的区别：

> 1. **授权协议的不同：** OpenJDK采用GPL V2协议放出，而SUN JDK则采用JRL放出。两者协议虽然都是开放源代码的，但是在使用上的不同在于GPL V2允许在商业上使用，而JRL只允许个人研究使用。
>
> 2. **OpenJDK源代码不完整：** 这个很容易想到，在采用GPL协议的OpenJDK中，SUN JDK的一部分源代码因为产权的问题无法开放给OpenJDK使用
>
> 3. **部分源代码用开源代码替换：** 由于产权的问题，很多产权不是SUN的源代码被替换成一些功能相同的开源代码，比如说字体栅格化引擎，使用Free Type代替。
>
> 4. **OpenIDK只包含最精简的JDK：** OpenJDK不包含其他的软件包，比如Rhino Java DB JAXP……，并且可以分离的软件包也都是尽量的分离，但是这大多数都是自由软件，你可以自己下载加入。
>
>    不能使用Java商标：这个很容易理解，在安装OpenJDK的机器上，输入“java-version”显示的是OpenJDK，但是如果是使用Icedtea补丁的OpenJDK，显示的是java。
>
> --摘自知乎《OpenJDK和SunJDK有啥区别？》

好啦！大功告成！！！！