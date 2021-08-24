# Ansible for ISUCON

## host.ymlを作成する

```yml:hosts.yml
all: # グループ名
  hosts:
    aws1: # ホスト名
      ansible_host: ec2-52-194-220-6.ap-northeast-1.compute.amazonaws.com # 対象アドレス
      ansible_user: ubuntu
      ansible_ssh_private_key_file: ~/.ssh/id_rsa_aws.pem
```

この状態で、以下のコマンドを用いて疎通確認が行える。
`-i`でhostsファイルを指定し、`-m`で利用するモジュールを指定し、対象のグループ名を続けて指定する。

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

## playbookを実行する

```sh
ansible-playbook playbook.yml
```
