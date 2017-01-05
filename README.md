1.实现将端口和进程号抓取出来，并且构造成json格式

2.zabbix监控已zabbix用户来运行，zabbix用户用netstat没权限，用ss命令则无法显示出进程号

3.zabbix监控的原理是读取/proc/net/tcp这个文件，从中获取监听的那条记录,所以和端口开发的ip无关

4.通过ss命令实现，netstat会被淘汰，ss命令更快，更详细

4.读取/proc/net/tcp这个文件经常不准，发现是内核会频繁的写这个文件，导致读取不正常解决方案是在zabbix下配置一条自定义的监控项，取代默认的项：
UserParameter=net.tcp.listen.grep[*],grep -q $$(printf '%04X.00000000:0000.0A' $1) /proc/net/tcp && echo 1 || echo 0 
