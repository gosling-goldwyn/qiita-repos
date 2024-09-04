---
title: DockerでPortainerを動かす
tags:
  - Docker
  - portainer
private: false
updated_at: '2021-02-20T14:16:44+09:00'
id: de158ea332bebbcf3c52
organization_url_name: null
slide: false
ignorePublish: false
---
## portainerって何？

Docker環境管理ツールです
https://www.portainer.io/

## 対象

Dockerコマンド打つのめんどくさい…
sshするのめんどくさい…
的な人向け

## 前提

Dockerのインストールは済ませておくこと

## インストール

```bash
$ docker run \
    --detach \
    --publish 9000:9000 \
    --name portainer \
    --restart always \
    --volume /var/run/docker.sock:/var/run/docker.sock \
    --volume portainer_data:/data \
    portainer/portainer-ce
```

## 確認

`http://localhost:9000`にアクセスし、管理画面が出てくれば成功
