# 手順
1. .envファイルを作成し、以下の項目を設定する
* ELASTIC_PASSWORD
* KIBANA_PASSWORD
```
# Password for the 'elastic' user (at least 6 characters)
ELASTIC_PASSWORD=

# Password for the 'kibana_system' user (at least 6 characters)
KIBANA_PASSWORD=

# Version of Elastic products
STACK_VERSION=8.6.1

# Set the cluster name
CLUSTER_NAME=docker-cluster

# Set to 'basic' or 'trial' to automatically start the 30-day trial
LICENSE=basic
#LICENSE=trial

# Port to expose Elasticsearch HTTP API to the host
ES_PORT=9200
#ES_PORT=127.0.0.1:9200

# Port to expose Kibana to the host
KIBANA_PORT=5601
#KIBANA_PORT=80

# Increase or decrease based on the available host memory (in bytes)
MEM_LIMIT=1073741824

# Project namespace (defaults to the current folder name if not set)
#COMPOSE_PROJECT_NAME=myproject
```
2. docker-compose upする
3. http://localhost:5601にアクセス
```
user: elastic
pass: .envのELASTIC_PASSWORDに設定した値
```
4. Dev Toolsからクラスターのステータスを確認
```
GET _cluster/health
```
5. ホスト側からhttpsでcurlするために、esのコンテナから証明書を取ってくる
```
docker cp es-kibana-local-es01-1:/usr/share/elasticsearch/config/certs .
```

6. ホストからcurlする
```
curl --cacert ./certs/ca/ca.crt -u elastic:[.envのELASTIC_PASSWORDに設定した値] -X GET "https://localhost:9200/_cluster/health?pretty"
```

# 参考リンク
* https://www.elastic.co/guide/en/elasticsearch/reference/current/docker.html