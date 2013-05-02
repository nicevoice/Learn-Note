##安装MySQL
说明：在两台MySQL服务器192.168.21.169和192.168.21.168上分别进行如下操作，安装MySQL 5.5.22

##配置MySQL主服务器（192.168.21.169）
`mysql -uroot -p`   #进入MySQL控制台

`create database osyunweidb;`   #建立数据库osyunweidb

`insert into mysql.user(Host,User,Password) values('localhost','osyunweiuser',password('123456'));`   #创建用户osyunweiuser
#建立MySQL主从数据库同步用户osyunweidbbak密码123456 

`flush privileges;`    #刷新系统授权表

#授权用户osyunweidbbak只能从192.168.21.168这个IP访问主服务器192.168.21.169上面的数据库，并且只具有数据库备份的权限
`grant replication slave  on *.* to 'osyunweidbbak'@'192.168.21.168' identified by '123456' with grant option; `

##把MySQL主服务器192.168.21.169中的数据库osyunweidb导入到MySQL从服务器192.168.21.168中
###导出数据库osyunweidb
`mysqldump -u root -p osyunweidb > /home/osyunweidbbak.sql  ` #在MySQL主服务器进行操作，导出数据库osyunweidb到/home/osyunweidbbak.sql 
备注：在导出之前可以先进入MySQL控制台执行下面命令
```bash
flush tables with read lock;   #数据库只读锁定命令，防止导出数据库的时候有数据写入
unlock tables;  #解除锁定
```
###导入数据库到MySQL从服务器
```bash
mysql -u root -p #进入从服务器MySQL控制台
create database osyunweidb;   #创建数据库
use osyunweidb   #进入数据库
source /home/osyunweidbbak.sql #导入备份文件到数据库
mysql -u osyunweidbbak -h 192.168.21.169 -p #测试在从服务器上登录到主服务器
```

##配置MySQL主服务器的my.cnf文件
```bash
vi /etc/my.cnf   #编辑配置文件，在[mysqld]部分添加下面内容
server-id=1  #设置服务器id，为1表示主服务器，注意：如果原来的配置文件中已经有这一行，就不用再添加了。
log_bin=mysql-bin #启动MySQ二进制日志系统，注意：如果原来的配置文件中已经有这一行，就不用再添加了。
binlog-do-db=osyunweidb #需要同步的数据库名，如果有多个数据库，可重复此参数，每个数据库一行
binlog-ignore-db=mysql  #不同步mysql系统数据库
service mysqld restart #重启MySQL
mysql -u root -p  #进入mysql控制台
show master status; 
```
查看主服务器，出现以下类似信息
+------------------+--------------+-----------------+------------------+
|File              | Binlog_Do_DB | Binlog_Ignore_DB|                  |
+------------------+--------------+-----------------+------------------+
|mysql-bin.000019  |   7131       |osyunweidb       |mysql             |
+------------------+--------------+-----------------+------------------+
1 row in set (0.00 sec)
注意：这里记住File的值：`mysql-bin.000019`和Position的值：`7131`，后面会用到。

##配置MySQL从服务器的my.cnf文件
```bash
vi /etc/my.cnf  #编辑配置文件，在[mysqld]部分添加下面内容
server-id=2  #配置文件中已经有一行server-id=1，修改其值为2，表示为从数据库
log-bin=mysql-bin #启动MySQ二进制日志系统，注意：如果原来的配置文件中已经有这一行，就不用再添加了。
replicate-do-db=osyunweidb  #需要同步的数据库名，如果有多个数据库，可重复此参数，每个数据库一行
replicate-ignore-db=mysql  #不同步mysql系统数据库
```
:wq!   #保存退出
`service mysqld restart`  #重启MySQL
注意：MySQL5.1.7版本之后，已经不支持把master配置属性写入my.cnf配置文件中了，只需要把同步的数据库和要忽略的数据库写入即可。
```bash
mysql -u root -p #进入MySQL控制台
slave stop;  #停止slave同步进程
change master to
master_host='192.168.21.169',master_user='osyunweidbbak',master_password='123456',master_log_file='mysql-bin.000019' ,master_log_pos=7131;   #执行同步语句
slave start;   #开启slave同步进程
SHOW SLAVE STATUS\G  #查看slave同步信息，出现以下内容
```
***************************1. row ***************************
            Slave_IO_State: Waiting for master to send event
                 Master_Host: 192.168.21.169
                 Master_User: osyunweidbbak
                 Master_Port: 3306
               Connect_Retry: 60
             Master_Log_File: mysql-bin.000019
         Read_Master_Log_Pos: 7131
              Relay_Log_File:MySQLSlave-relay-bin.000002
               Relay_Log_Pos: 253
       Relay_Master_Log_File: mysql-bin.000019
            Slave_IO_Running:Yes
           Slave_SQL_Running: Yes
             Replicate_Do_DB: osyunweidb
         Replicate_Ignore_DB: mysql
          Replicate_Do_Table:
      Replicate_Ignore_Table:
1 row in set (0.00 sec)
注意查看：
***Slave_IO_Running: Yes***
***Slave_SQL_Running: Yes***
以上这两个参数的值为Yes，即说明配置成功！

##测试MySQL主从服务器双机热备是否成功
###进入MySQL主服务器
mysql -u root -p #进入主服务器MySQL控制台
use osyunweidb  #进入数据库
CREATE TABLE test ( id int not null primary key,name char(20));  #创建test表
###进入MySQL从服务器
mysql -u root -p #进入MySQL控制台
use osyunweidb  #进入数据库
show tables; #查看osyunweidb表结构，会看到有一个新建的表test，表示数据库同步成功

至此，MySQL数据库配置主从服务器实现双机热备实例教程完成
