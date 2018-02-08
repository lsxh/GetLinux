### Deepin操作系统使用dpkg包管理，这里简单一下安装 g、g++以及它们的使用

深度操作系统使用dpkg包管理. 我们除了通过常用的深度商店、Synaptic等图形软件管理工具外，也可以通过命令对软件包进行安装、卸载与系统升级等日常管理。

***dpkg包管理在[基础->包管理](../../../knowledge-base/Package-Management/list.md)部分有介绍，这里不赘述***

下面我们介绍如何在深度操作系统下使用命令管理软件包,管理功能包括:安装/管理单个软件包/升级软件套件

* 升级软件套件：sudo apt-get update
* 升级软件套件：sudo apt-get upgrade
* 安装g：sudo apt-get install g
* 安装g++:sudo apt-get install g++
* 卸载一个已安装的软件包（保留配置文件）:apt-get remove package_name

apkg包管理的其他相关知识详见[apkg包管理](https://wiki.deepin.org/index.php?title=软件包管理#.E5.89.8D.E8.A8.80)