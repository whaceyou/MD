```bash
docker run \
-p 3306:3306 \
--name mysql \
-v /mydata/mysql/log:/var/1og/mysql \
-v /mydata/mysql/data:/var/lib/mysql \
-v /mydata/mysql/conf:/etc/mysql \
-e MYSQL_ROOT_PASSWORD=poco1024! \
-d mysql:5.7
```

```bash
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

