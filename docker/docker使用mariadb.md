###  下载docker镜像

>  docker pull mariadb 
>
> docker images

###  创建数据卷

> docker volume create mariadb-vol

###  启动容器

> docker run --name mariadb -p 3306:3306 
>
> -e MYSQL_ROOT_PASSWORD=创建新的密码
>
> -v  mariadb-vol:/var/lib/mysql -d mariadb

> --name启动容器设置容器名称为mariadb
>
> -p设置容器的3306端口映射到主机3306端口
>
> -e MYSQL_ROOT_PASSWORD设置环境变量数据库root用户密码为输入数据库root用户的密码
>
> -v设置容器目录/var/lib/mysql映射到本地目录/data/mariadb/data
>
> -d后台运行容器mariadb并返回容器id

### 查看容器是否运行

> docker ps -a
>
> docker container update --restart=always 容器id  修改容器为自启动

###  进入容器

> docker exec -it 容器Id或者名字 bash

### 修改远程登录j及密码

####  1、设置远程登录：

> MariaDB [(none)]> GRANT ALL PRIVILEGES ON *.* TO root@"%" IDENTIFIED BY "root";
>
> MariaDB [(none)]> flush privileges;
>
> > 
>
>  GRANT ALL PRIVILEGES ON *.*TO 'root'@'%' IDENTIFIED BY 'root' WITH GRANT OPTION;
>
> 第一个* 表示数据库 #第二个* 表示权限 #% 表示的是所有的ip #只给用户一个cas的数据库 GRANT ALL PRIVILEGES ON cas.*TO 'cas'@'%' IDENTIFIED BY 'cas' WITH GRANT OPTION; flush privileges;

####  2、修改用户密码，以root为例

* 知道root密码，需要修改

> /*第一个方式：直接编辑数据库字段*/  
> MariaDB [(none)]> use mysql;  
> MariaDB [mysql]> UPDATE user SET password=password('newpassword') WHERE user='root';  
> MariaDB [mysql]> flush privileges;  
> MariaDB [mysql]> exit  

> /*第二个方式：修改密码，不用进入mysql*/  
> MariaDB [(none)]> SET password for 'root'@'localhost'=password('newpassword');  
> MariaDB [(none)]> exit;  

- 忘记密码：

> systemctl stop mariadb   先停掉当前的mysql进程，不然执行下一步说进程已经存在

> mysqld_safe --skip-grant-tables &  后台直接这个mysql，界面中还会出现日志，直接ctrl+c进入命令行输入

> ps -ef | grep mariadb   看进程，会突出显示--skip-grant-tables

>   mysql     3607  3368  0 18:05 pts/0    00:00:00 /usr/libexec/mysqld --basedir=/usr --datadir=/var/lib/mysql   
>   --plugin-dir=/usr/lib64/mysql/plugin --user=mysql --skip-grant-tables --log-error=/var/log/mariadb/mariadb.log   
>   --pid-file=/var/run/mariadb/mariadb.pid --socket=/var/lib/mysql/mysql.sock  
>
> mysql /*直接进入mysql，不需要密码等，执行第一步中方法a里两种方式中任何一种即可*/  
>
> MariaDB [(none)]> use mysql;  
> MariaDB [mysql]> UPDATE user SET password=password('newpassword') WHERE user='root';  
> MariaDB [mysql]> flush privileges;   
> MariaDB [mysql]> exit; /*这个时候用参数--skip-grant-tables启动的mysql已经会要求输入密码才能进入了*/  

