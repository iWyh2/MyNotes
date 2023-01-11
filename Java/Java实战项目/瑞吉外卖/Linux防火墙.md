# Linux防火墙操作



* 查看防火墙状态：systemctl status firewalld / firewall-cmd --state
* 暂时关闭防火墙：systemctl stop firewalld
* 永久关闭防火墙：systemctl disablefirewalld
* 开启防火墙：systemctl start firewalld
* 开放指定端口：firewall-cmd --zone=public --add-port=8080/tcp --permanent
* 关闭指定端口：firewall-cmd --zone=public --remove-port=8080/tcp --permanent
* 立即生效：firewall-cmd --reload
* 查看开放的端口：firewall-cmd --zone=public --list-ports



：systemctl是管理Linux中服务的命令，可以对服务进行启动 停止 关闭 重启查看状态等操作

：firewall-cmd是Linux中专门用于操作防护墙的命令