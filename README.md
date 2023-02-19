# es-kibana-local
ElasticsSearchとkibanaをlocalで動かしてみるやつ

## 環境
- Mac(M1)
- OS: Mac Ventura 13.1
- Docker version 20.10.17, build 100c701 

## 環境構築手順
参考リンクの通りに進める。  

※ ターミナルのウインドウを2個使います  
※ es初回起動時にkibana用の各認証情報がターミナルに出力されるので手元に控えること。  
(credentails-sample.mdを参考)  
※ 生成されるenrollment tokenは2種類あるのでkibanaログイン時には1つ目のtokenを使うこと

## kibanaのコンソールからリクエストする方法(ヘルスチェック)
1. サイドメニュー -> Management -> Dev Toolsを辿ってエディタを開く
    - http://0.0.0.0:5601/app/dev_tools#/console
1. 以下を入力して実行
    ```
    GET _cluster/health
    ```

## MacのローカルからCurlでリクエストする方法
1. elastic searchコンテナから、ローカルへhttps証明書をコピー  
    (es version8から、接続がデフォルトでhttpsになっているから)
    ```
    ex:
    $ docker cp <CoontainerID>:/usr/share/elasticsearch/config/certs/http_ca.crt .
    ```
1. コピーした証明書のパスを指定してcurlを実行(kibanaログインに使ったパスワードが必要)
    ```
    $ curl --cacert elasticsearch/certs/http_ca.crt -u elastic -X GET "https://localhost:9200/_cluster/health?pretty"

    返り値:
    Enter host password for user 'elastic':
    {
        "cluster_name" : "docker-cluster",
        "status" : "yellow",
        "timed_out" : false,
        "number_of_nodes" : 1,
        "number_of_data_nodes" : 1,
        "active_primary_shards" : 12,
        "active_shards" : 12,
        "relocating_shards" : 0,
        "initializing_shards" : 0,
        "unassigned_shards" : 1,
        "delayed_unassigned_shards" : 0,
        "number_of_pending_tasks" : 0,
        "number_of_in_flight_fetch" : 0,
        "task_max_waiting_in_queue_millis" : 0,
        "active_shards_percent_as_number
    }
    ```

## 参考リンク:
- https://github.com/elastic/elasticsearch  
- https://mebee.info/2022/10/27/post-83923/#outline__4
- https://zenn.dev/ktoyod/articles/803b9f1b2860b6

