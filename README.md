# Profiling tools for ISUCON

ISUCON でプロファイリングするためのツールをセットアップする Ansible role をまとめたリポジトリ

## host.yml を作成する

```yml:hosts.yml
all:
  hosts:
    host1:
      ansible_host: target.host.name
      ansible_user: ubuntu
      ansible_ssh_private_key_file: ~/.ssh/id_rsa_aws.pem
```

この状態で、以下のコマンドを用いて疎通確認が行える。
`-i`で hosts ファイルを指定し、`-m`で利用するモジュールを指定し、対象のグループ名を続けて指定する。

```sh
ansible -i hosts.yml -m ping all
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

## playbook を実行する

```sh
ansible-playbook playbook.yml
```
