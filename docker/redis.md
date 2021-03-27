```bash
mkdir -p /mydata/redis/conf
touch /mydata/redis/conf/redis.conf
docker run \
-p 6379:6379 \
--name redis \
-v /mydata/redis/conf/redis.conf:/etc/redis/redis.conf \
-v /mydata/redis/data:/data \
-d redis redis-server /etc/redis/redis.conf
```
