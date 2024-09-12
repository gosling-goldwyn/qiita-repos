---
title: DockerをAPIやSDKで操作できるようにする
tags: 
  - docker
private: false
updated_at: ''
id: null
organization_url_name: null
slide: false
ignorePublish: false
---
# DockerをAPIやSDKで操作できるようにする

※Dockerのアップデートで設定がデフォルトに戻るため注意してください

https://takuya-1st.hatenablog.jp/entry/2021/05/31/142017



```
sudo vim /lib/systemd/system/docker.service
```

該当サービスの ExecStartに -H tcp://0.0.0.0:2375 を追記する。

```
ExecStart=/usr/bin/dockerd -H fd:// --containerd=/run/containerd/containerd.sock -H tcp://0.0.0.0:2375
```

systemd をリロードして docker を再起動する

```
sudo systemctl daemon-reload
sudo systemctl restart docker
```

これでTCP経由で接続ができる。
