```bash
docker run -p 6379:6379 --name redis -v /mydata/redis/data:/data \
-v /mydata/redis/conf/redis.conf:/etc/redis/redis.conf\
-d redis redis-server /etc/redis/redis.conf
```

```bash
mkdir -p /mydata/redis/conf
touch /mydata/redis/conf/redis.conf

```

```bash
docker update redis --restart=always
```

