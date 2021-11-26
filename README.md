[![CI](https://github.com/ktooi/ansible-role-poweruser/workflows/CI/badge.svg)](https://github.com/ktooi/ansible-role-poweruser/actions?query=workflow%3ACI+branch%3Amain)

# Ansible Role: poweruser

`ansible_user` で Ansible が接続できない場合に、(ディストリビューションごとに設定されたり、チームや個人で決めた)デフォルトのユーザを利用して Ansible が利用するユーザを作成する role です。

この role では次のことを行います。

* `ansible_user` で接続可能か確認する。
* `ansible_user` で接続できない場合、 `power_default_*` の認証情報を利用して接続し、 `ansible_user` を作成・設定する。

この role では次のことを行いません。

* `power_default_user` の削除。

## Requirements

この role には特別な要件はありません。

## Role Variables

[defaults/main.yml](defaults/main.yml) を参照してください。


## Dependencies

None.

## Example Playbook

```yaml
- hosts: all
  gather_facts: false
  roles:
    - ktooi.poweruser
  vars:
    # for Raspberry pi OS
    power_default_user: pi
    power_default_ssh_pass: raspberry
    power_default_become_pass: raspberry
```

※ `gather_facts` は `false` にする必要があります。対象ホストにログインができない場合には、 `gather_facts` を実行しても失敗するためです。

## Authors

* **Kodai Tooi** [GitHub](https://github.com/ktooi), [Qiita](https://qiita.com/ktooi)

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details
