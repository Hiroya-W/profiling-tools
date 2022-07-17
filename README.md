# Profiling tools for ISUCON

ISUCON でプロファイリングするためのツールをセットアップする Ansible role をまとめたリポジトリです。

主に以下のツールをセットアップします。

- [tkuchiki/alp](https://github.com/tkuchiki/alp) Access Log Profiler
- [percona/percona-toolkit](https://github.com/percona/percona-toolkit) collection of advanced command-line tools used by Percona support staff to perform a variety of MySQL and system tasks that are too difficult or complex to perform manually
- [wan-nyan-wan/discocat](https://github.com/wan-nyan-wan/discocat) simple commandline utility to post snippets to Discord

## host.yml を作成する

`localhost` をターゲットとする場合は、以下のようにする。

```yml:hosts.yml
all:
  hosts:
    host1:
      ansible_connection: local
      ansible_host: localhost
      ansible_user: ubuntu
```

Remoteのホストとして `hoge.fuga.jp` をターゲットとする場合は、以下のようにする。

```yml:hosts.yml
all:
  hosts:
    host1:
      ansible_host: hoge.fuga.jp
      ansible_user: ubuntu
      ansible_ssh_private_key_file: ~/.ssh/id_rsa_host1.pem
```

この状態で、以下のコマンドを用いて疎通確認が行える。
`-i`で hosts ファイルを指定し、`-m`で利用するモジュールを指定し、対象のグループ名を続けて指定する。

```sh
ansible -i inventory/hosts.yml -m ping all
```

```
host1 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
```

## discocat 用の var を記述する

```
vim inventory/group_vars/all.yml
```

```yml:inventory/group_vars/all.yml
---
discocat_bot_token: BOT_TOKEN_HERE
discocat_channel_id: CHANNEL_ID_HERE
```

## playbook を実行する

```sh
ansible-playbook playbook.yml
```
