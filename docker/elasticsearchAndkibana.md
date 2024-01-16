# elasticsearch
## 拉取

```shell
docker pull elasticsearch:7.4.2
docker pull kibana:7.4.2
```

## 目录挂载

```shell
mkdir -p /mydata/elasticsearch/config
mkdir -p /mydata/elasticsearch/plugins
mkdir -p /mydata/elasticsearch/data

echo "http.host:0.0.0.0">>/mydata/elasticsearch/config/elasticsearch.yml
```

## 启动
```shell
chmod -R 777 /mydata/elasticsearch
```
```shell
docker run --name elasticsearch -p 9200:9200 \
-p 9300:9300 \
-e "discovery.type=single-node" \
-e ES_JAVA_OPTS="-Xms64m -Xmx512m" \
-v /mydata/elasticsearch/config/elasticsearch.yml:/usr/share/elasticsearch/config/elasticsearch.yml \
-v /mydata/elasticsearch/data:/usr/share/elasticsearch/data \
-v /mydata/elasticsearch/plugins:/usr/share/elasticsearch/plugins \
-d elasticsearch:7.4.2
```

# kibana
```shell
docker run --name kibana \
-e ELASTICSEARCH_HOSTS=http://192.168.12.19:9200 \
-p 5601:5601 \
-d kibana:7.4.2
```
