# Linux项目部署



## 手工部署项目

1. 在IDEA中开发好SpringBoot程序，最后打好jar包
2. 将Windows中打好的jar包上传到Linux中（一般是放在 /usr/local/app目录中）
3. 修改所需配置（application.yml）到jar包同级config目录中
4. 执行 java -jar “jar包名”

这样的执行有一个问题就是会霸屏控制台，且终端退出程序就终止了

所以我们需要将项目后台运行，并输出日志到指定日志文件中

：nohup命令（no hang up，不挂断的运行指定的命令，终端退出也不会有影响）

-> 语法：nohup Command [Arg .. ] [&]

* Command：要执行的命令
* Arg：一些参数，可以指定输出文件
* &：让命令在后台运行

例如：nohup java -jar “jar包名” &>日志文件名.log &

其中“日志文件名.log”是一个相对路径 会在当前文件夹中生成这样的一个文件



停止项目：找到项目进程id，kill -9



## 通过Shell脚本自动部署项目

具体流程：

本地开发环境（开发项目上传到Git仓库）

Linux服务器（从Git仓库拉取项目，然后编译 打包 启动）

* 使用Git克隆代码
  * cd/usr/local/
  * git clone xxxxxxxx(Git仓库地址)
* 本机Linux的shell脚本模板存放位置：/usr/local/sh



操作步骤：

1. 在Linux中安装Git
2. 在Linux中安装Maven
3. 编写shell脚本（拉取代码 编译 打包 启动）（或者直接使用现成的脚本模板）
4. 为用户授予执行shell脚本的权限
5. 执行shell脚本



