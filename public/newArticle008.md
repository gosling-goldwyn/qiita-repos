---
title: dockerプライベートリポジトリにカスタムイメージをpushする方法
tags:
  - 'Docker'
  - 'Harbor'
private: false
updated_at: ''
id: null
organization_url_name: null
slide: false
ignorePublish: true
---
# dockerプライベートリポジトリにカスタムイメージをpushする方法

## PushしたいDockerの設定

pushしたいdocker側のconfig(`/etc/docker/daemon.json`)をいじる

```:/etc/docker/daemon.json
{
 "insecure-registries": ["my-docker-registry:5000"]
}
```

その後

```
sudo systemctl restart docker
```

Docker for windowsの場合は`C:\ProgramData\docker\config\daemon.json`を作成して、サービスからdockerを再起動する


## DockerでPush

コマンドラインでharborにログインしてpushする

```bash
$ docker login -u username -p password hostname
$ docker tag IMAGENAME[:TAG] hostname/library/IMAGENAME[:TAG]
$ docker push hostname/library/IMAGENAME[:TAG]

# example
$ docker tag buildtools2019:latest hostname/library/buildtools2019:latest
$ docker push hostname/library/buildtools2019:latest
```

プライベートリポジトリに反映されていたら成功