一 安装mysql

1.下载mysql的repo源

[root@localhost ~]# wget http://repo.mysql.com/mysql-community-release-el7-5.noarch.rpm 
2.安装mysql-community-release-el7-5.noarch.rpm包

[root@localhost ~]# sudo rpm -ivh mysql-community-release-el7-5.noarch.rpm  
3.安装mysql，根据提示安装就可以了,不过安装完成后没有密码,需要重置密码

[root@localhost ~]# sudo yum install mysql-server         
4.重置mysql密码

[root@localhost ~]# mysql -u root 
5.此时应该会抛错，大概是没有权限访问，这里修改为当前用户

  [root@localhost ~]# sudo chown -R root:root /var/lib/mysql
6.重启mysql服务

  [root@localhost ~]# $ service mysqld restart 
7.进入mysql，重置密码

  [root@localhost ~]# mysql -u root         
    mysql > use mysql;
    mysql > update user set password=password('123456') where user='root';
    mysql> grant all privileges on *.* to 'root'@'%' identified by '123456' with grant option;   (授权远程可连接)
    mysql> flush privileges;  （让权限生效）
    mysql > exit;
二 安装reids

安装redis,启动redis

[root@localhost ~]# yum install redis  
[root@localhost ~]# service redis start
如果redis安装在云服务器上面，你想通过远程连接，以后方便查看的话，你可以找到redis的配置文件，如果不知道配置文件在哪里，可以通过这个命令来找：

[root@localhost ~]# find / -name "redis*" 
我们找到一个redis.conf的文件（默认在/etc/redis.conf下）
然后使用vi或vim打开此文件，这里我们需要注意的有这么几个地方：
1.如果想远程访问，可以找到 #bind 127.0.0.1 改为 bind (你的ip地址)
如果你经常换ip的话，可以改为 bind 0.0.0.0 (不建议，因为允许了所有的人连接，很容易被攻击)
2.找到#requirepass foobared,改为 requirepass 123456，这样，连接redis就需要密码了，密码是123456
3.daemonize设置为yes，这样就表示可以后台启动redis