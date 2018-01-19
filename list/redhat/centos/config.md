# CentOS 的相关配置

> CentOS 安装完毕后，进行一些配置是有必要的，

通过本文，你将了解一下内容：

1. CentOS 安装后的简单配置
1. 安装LAMP/LNMP环境
    > 作为一个PHP程序员，需要懂得在Linux环境下安装环境，这些都是一些基本的技能！现在让我们手动的来安装吧，下面我们将使用两种方式来安装我们的环境，即通过在线安装和编译安装进行我们的操作！你准备好了吗？

    **在线安装：** 在线软件仓库的使用，给我们带来许多便利，这种模式使得我们不再需要将时间花费在手动编译的等待上，而是直接使用已经编译好的rpm包来安装，省时省力。下面我们在CentOS7下安装Apache + PHP + MySQL

    ```bash
    #安装Apache
        sudo yum install httpd
    #安装MySQL
        sudo yum install mysql
    #安装PHP
        sudo yum install php
    ```

1. 安装Oracle JDK