[![CI](https://github.com/ktooi/ansible-role-poweruser/workflows/CI/badge.svg)](https://github.com/ktooi/ansible-role-poweruser/actions?query=workflow%3ACI+branch%3Amain)

# Ansible Role: poweruser

この role は、 Ansible が各サーバに接続するために利用するユーザ(`ansible_user`)を作成するための role です。

`ansible_user` で Ansible が接続できない場合に、(ディストリビューションごとに設定されたり、チームや個人で決めた)デフォルトのユーザを利用して Ansible が利用するユーザを作成します。

この role では次のことを行います。

* `ansible_user` で接続可能か確認する。
* `ansible_user` で接続できない場合、 `poweruser_default_*` の認証情報を利用して接続し、 `ansible_user` を作成・設定する。

この role では次のことを行いません。

* `poweruser_default_user` の削除。

## Requirements

この role には、 Ansible を実行するホスト上に sshpass がインストールされている必要があります。

### Ubuntu

Ubuntu 20.04 の場合は apt でインストール可能です。

```shell-session
$ sudo apt update
$ sudo apt install sshpass
```

### RHEL 系

RHEL7 の場合は EPEL からインストール可能です。

```shell-session
$ sudo yum install epel-release
$ sudo yum update
$ sudo yum sshpass
```

## Role Variables

[defaults/main.yml](defaults/main.yml) を参照してください。

## Dependencies

None.

## Example Playbook

```
[all]
my_host.example.com

[all:variables]
ansible_user: my_poweruser
ansible_ssh_pass: my_poweruser_password
```

```yaml
- hosts: all
  gather_facts: false
  roles:
    - ktooi.poweruser
  vars:
    # for Raspberry pi OS
    poweruser_default_user: pi
    poweruser_default_ssh_pass: raspberry
    poweruser_default_become_pass: raspberry
```

ポイント

* `gather_facts` は `false` にする必要があります。対象ホストにログインができない場合には、 `gather_facts` を実行しても失敗するためです。
* 例示している `poweruser_default_*` の認証情報は Raspberry Pi OS のデフォルトユーザの認証情報ですが、現行の Raspberry Pi OS では利用できないようになっているのでご注意ください。[^1]

[^1]: [An update to Raspberry Pi OS Bullseye - Raspberry Pi](https://www.raspberrypi.com/news/raspberry-pi-bullseye-update-april-2022/)

この Playbook は次のように動きます。

1. `my_poweruser` というユーザと `my_poweruser_password` というパスワードで `my_host.example.com` に接続します。
2. 1.の接続に失敗した場合、 `poweruser_default_*` の認証情報を利用して `my_poweruser` を作成します。上記例の場合は `pi` というユーザと `raspberry` というパスワードで `my_host.example.com` に接続し、 `my_poweruser` を作成します。
3. 以降の Playbook では `my_poweruser` というユーザを利用して `my_host.example.com` に接続します。

## Example usage


## Authors

* **Kodai Tooi** [GitHub](https://github.com/ktooi), [Qiita](https://qiita.com/ktooi)

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details
