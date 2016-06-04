Role Name
=========

A role for user management.

Requirements
------------

* Root privileges (for creating/removing/updating users or groups)

Role Variables
--------------

本ロールで定義されている変数は以下の通りです。

| variable                  | description                                                       | default value              |
|---------------------------+-------------------------------------------------------------------+----------------------------|
| users_generic_user_map    | 全ユーザのリスト (詳細は後述)                                     | []                         |
| users_username_list       | 作成/削除するユーザのリスト (詳細は後述)                          | []                         |
| users_group_list          | 作成/削除するグループのリスト (詳細は後述)                        | []                         |
| users_extra_user_list     | users_username_list以外に作成/削除するユーザのリスト (詳細は後述) | []                         |
| users_extra_group_list    | users_group_list以外に作成/削除するグループのリスト (詳細は後述)  | []                         |
| users_authorized_keys_dir | 参照するauthorized_keysの配置場所                                 | "files/authorized_keys.d/" |


### users_generic_user_map

ansible管理下のサーバ共通で使用するユーザを定義します。
userモジュールと同じパラメータ(一部未定義)名のものは、値をそのままuser
モジュールに渡します。使用可能なパラメータは下表の通りです。

| key             | required | default value |
|-----------------+----------+---------------|
| name            | yes      |               |
| uid             | yes      |               |
| comment         | no       |               |
| state           | no       | present       |
| group           | yes      |               |
| groups          | no       |               |
| append          | no       | no            |
| createhome      | no       | yes           |
| force           | no       |               |
| home            | no       |               |
| move_home       | no       | no            |
| password        | no       |               |
| remove          | no       | no            |
| shell           | no       |               |
| update_password | no       | always        |
| sudo            | no       | no            |
| authorized_keys | no       | no            |

※ authorized_keysがyesの場合、users_authorized_keys_dir以下のユーザ名
   のファイルの中身をauthorized_keysとして配置します。

#### Example users_generic_user_map

```
users_generic_user_list:
  - { name: test1, uid: 1200, group: test1,  authorized_keys: "yes", sudo: "yes" }
  - { name: test2, uid: 1202, group: test2,  groups: wheel, comment: "test2 user" }
  - { name: test3, uid: 1203, group: test3,  groups: wheel, comment: "test3 user", authorized_keys: "no" }
```

### users_username_list

作成/削除するユーザのリストを定義します。users_generic_user_listに記載
のユーザ名のみ指定できます。

#### Example users_username_list

```
users_username_list:
  - test1
  - test2
  - test3
```

### users_group_list

作成/削除するグループのリストを定義します。

| key  | required | default value |
|------+----------+---------------|
| name | yes      |               |
| gid  | yes      |               |
| sudo | no       | no            |

#### Example users_group_list

```
users_group_list:
  - { name: admin, gid: 1101 }
  - { name: test1, gid: 1200, sudo: no }
  - { name: test2, gid: 1202, sudo: yes }
  - { name: test3, gid: 1203 }
```

### users_extra_user_list

users_generic_user_listに記載のないユーザを作成する場合に使用します。
フォーマットはusers_generic_user_listと同じです。そちらを参照してくだ
さい。

#### Example users_extra_user_list

```
users_extra_user_list:
  - { name: extrauser, uid: 1305, group: extragroup, comment: "extrauser" }
```

### users_extra_group_list

users_group_listに記載のないグループを作成する場合に使用します。
フォーマットはusers_group_listと同じです。そちらを参照してください。

#### Extrauser users_extra_group_list

```
users_extra_group_list:
  - { name: extragroup, gid: 1300 }
```


Dependencies
------------


Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: servers
      roles:
         - users
