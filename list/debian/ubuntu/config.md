# 配置

## 设置 root 用户

debian系列（如Ubuntu、deepin、debian）安装过程都不用设置 root 用户，默认是没有 root 登陆的，需要使用 su root 命令来进入 root 用户。首次使用我们需要先设置 root 密码，才能 root登陆。

- 首先 鼠标右键 打开终端，设置密码。
 ```bash
    sudo passwd root
```
输入当前用户的密码,并设置 root 用户密码
 ```bash
    su root
```
使用刚设置的 roo 密码登陆

![img](../public/ubuntu/r-1.png)
 ```bash
    exit
```
使用 exit 命令退出root用户。

## 语言切换

有些 Ubuntu 的版本并没有安装所有的语言，所以安装好 Ubuntu 还需要自己安装一下。

- 打开软件-->搜索 language -->找到 language Support-->点击图示图标

    ![language](../public/ubuntu/2-2.png)
    ![language](../public/ubuntu/2-1.png)

- 点开后你会发现可选的语言不多，然后你就可以安装你自己需要的语言。

    ![img](../public/ubuntu/2-3.png)
    ![img](../public/ubuntu/2-4.png)

- 安装好后将你需要的语言拖到第一位，重启即可。（过程中要输入的密码就是你Ubuntu的登录密码）

    ![img](../public/ubuntu/2-5.png)

## other配置