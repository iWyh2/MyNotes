# Linux软件安装



## 二进制包发布包安装

软件已经针对具体平台编译打包发布，只需要解压，修改配置即可



## rpm安装

软件按照Redhat的包管理规范进行打包，使用rpm命令进行安装，不能自行解决库依赖问题

RPM：Red-Hat Package Manager

RPM软件包管理，是红帽Linux用于管理和安装软件的工具



## yum安装

一种在线安装软件的方式，本质上还是rpm安装，自动下载安装包并安装。安装过程中，自行解决库依赖问题

yum：Yellow Dog Updater，Modified

是一个在Fedora和RedHat以及CentOS中的Shell前端软件包管理器，基于RPM包管理。能够从指定的服务器自动下载RPM包并且安装，可以自动处理依赖关系，并且一次安装所有的依赖的软件包，无需一次次的下载安装



## 源码编译安装

软件以源码工程的形式发布，需要自己编译打包



利用FinalShell的文件拖拽上传文件进行安装

* 安装JDK
* 安装Tomcat
* 安装MySQL
* 安装lrzsz



