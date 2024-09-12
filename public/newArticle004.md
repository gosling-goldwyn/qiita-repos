---
title: 入力履歴を残さず社内プロキシを乗り越える
tags:
  - 'Ubuntu'
  - 'Linux'
private: false
updated_at: ''
id: null
organization_url_name: null
slide: false
ignorePublish: false
---
# プロキシ認証するには

Ubuntuでは環境変数にユーザー名とパスワードを記述する必要がある。

以下を設定するとプロキシ経由で外に行けます。
```sh
export http_proxy="<ユーザー名>:<パスワード>@proxy.hoge.com:port/"
export https_proxy="<ユーザー名>:<パスワード>@proxy.hoge.com:port/"
```
しかし、この設定だとパスワードを平文で保存してしまうのであんまりよくない。（共有のターミナルの場合、`printenv`コマンドですぐにユーザー名とパスワードを見れてしまう）

下記のシェルスクリプトを使って以下のコマンドを入力すれば、

```sh
sudo -i
./xxx.sh <コマンド>
```
とすればプロンプト上でユーザー名とパスワードを入力するのでよりセキュア（なはず）。

```sh:xxx.sh
unset password
unset name
prompt="Enter name:"
while IFS= read -p "$prompt" -r -n 1 char
do
    if [[ $char == $'\0' ]]
    then
        break
    fi
    prompt=""
    name+="$char"
done
prompt="Enter Password:"
while IFS= read -p "$prompt" -r -s -n 1 char
do
    if [[ $char == $'\0' ]]
    then
        break
    fi
    prompt='*'
    password+="$char"
done

function urlencode() {
    # urlencode <string>
    local length="${#1}"
    for (( i = 0; i < length; i++ )); do
        local c="${1:i:1}"
        case $c in
            [a-zA-Z0-9.~_-]) printf "$c" ;;
            *) printf '%%%02X' "'$c"
        esac
    done
}

encoded_name=$(urlencode $name)
encoded_pw=$(urlencode $password)

http_proxypath="http://$encoded_name:$encoded_pw@proxy.hoge.com:port/"
https_proxypath="http://$encoded_name:$encoded_pw@proxy.hoge.com:port/"

export http_proxy=$http_proxypath
export https_proxy=$https_proxypath

exec "$@"

unset http_proxy
unset https_proxy
```