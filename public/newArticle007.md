---
title: Elasticsearchがデータを取り込んでくれなくなったとき
tags:
  - Fluentd
  - Elasticsearch
private: false
updated_at: '2024-09-12T23:34:12+09:00'
id: 0d1070e494890b4f98a7
organization_url_name: null
slide: false
ignorePublish: false
---
# elasticsearchがデータを取り込んでくれなくなった


fluentdのelasticプラグインがエラーを吐いていた
詳細を見るにはfluentd側に以下を追加する必要があった
```
log_es_400_reason true
```

こんなエラーが出てた
```
Validation Failed: 1: this action would add [2] shards, but this cluster currently has [1000]/[1000] maximum normal shards open
```

どうやらelasticsearchのshardsがいっぱいになってたらしい
↓を参考にshardsの容量を変更

https://note.classact.co.jp/62ab05a5aaaf78236858f7a3#cluster.max_shards_per_node%20%E3%81%AE%E8%A8%AD%E5%AE%9A%E5%A4%89%E6%9B%B4

1000→10000にした
```
curl -X PUT -u user:password localhost:9200/_cluster/settings -H "Content-Type: application/json" -d '{ "persistent": { "cluster.max_shards_per_node": "10000" } }'
```
