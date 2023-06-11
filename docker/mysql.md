```bash
docker run \
-p 33080:3306 \
--name mysql80 \
-v /mydata/mysql80/data:/var/lib/mysql \
-v /mydata/mysql80/conf.d:/etc/mysql/conf.d \
-e MYSQL_ROOT_PASSWORD=poco1024! \
-e MYSQL_DEFAULT_AUTHENTICATION_PLUGIN=mysql_native_password \
-d mysql:8.0

docker run \
-p 33057:3306 \
--name mysql57 \
-v /mydata/mysql57/data:/var/lib/mysql \
-v /mydata/mysql57/conf.d:/etc/mysql/conf.d \
-e MYSQL_ROOT_PASSWORD=poco1024! \
-d mysql:5.7
```

```bash
cd /mydata/mysql/conf
vi my.cnf
[client]
default-character-set=utf8

[mysql]
default-character-set=utf8

[mysqlId]
init_connect='SET collation_connection = utf8_ unicode_ci'
init_connect='SET NAMES utf8'
character-set-server=utf8
collation-server=utf8_unicode_ci
skip-character-set-client-handshake
skip-name-resolve
```

