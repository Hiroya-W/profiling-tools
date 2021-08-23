# Ansible_tutorial

## host.ymlを作成する

```yml:hosts.yml
all: # グループ名
  hosts:
    aws1: # ホスト名
      ansible_host: ec2-52-194-220-6.ap-northeast-1.compute.amazonaws.com # 対象アドレス
      ansible_user: ubuntu
```

この状態で、以下のコマンドを用いて疎通確認が行える。
`-i`でhostsファイルを指定し、`-m`で利用するモジュールを指定し、対象のグループ名を続けて指定する。
また、AWS EC2への接続に利用する鍵を`--private-key`で指定する。

```sh
ansible -i hosts.yml -m ping all --private-key="$HOME/.ssh/id_rsa_aws.pem"
```

もしくは、`ssh-agent`を用いることで`--private-key`の入力を省略することも出来る。

```sh
# fish
eval (ssh-agent -c)
ssh-add ~/.ssh/id_rsa_aws.pem
```

```sh
ansible -i hosts.yml -m ping all
```

```
aws1 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}

```

## ansible.cfgに設定を記述

```ini
[defaults]
; inventoryの指定
inventory = ./hosts.yml
; roleを格納するパスの指定
roles_path = ./roles/
; 実行ログの場所を指定
log_path = ./ansible.log
; 対象ホストの情報を収集するかどうか
gathering = smart
; 実行失敗時にできるretryファイルを作らない
retry_files_enabled = False
```
