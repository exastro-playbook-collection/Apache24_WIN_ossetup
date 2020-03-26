# Ansible Role: Apache24_WIN\_ossetup

## Description
本Ansble RoleはWebサーバソフトウェアである"Apache HTTP Server"の環境設定を行います。
対象バージョンは以下のバージョンです。

- Apache2.4 (Apache Launge版)

以下の設定を行います。
- Apacheサービスの自動起動設定
- Apacheサービスの再起動

## Supports

- 管理マシン(Ansibleサーバ)
  * Linux系OS（RHEL/CentOS）
  * Ansible バージョン 2.3 以上 (動作確認済みバージョン：2.4、2.9)
  * Python バージョン 2.x、3.x  (動作確認済みバージョン：2.6、2.7、3.6)


- 管理対象マシン(インストール対象マシン)
  - Windows Server 2016
  - PowerShell 5.0

## Requirements

- 管理マシン(Ansibleサーバ)
  - 管理対象マシンへ	WinRM接続できること
  - WinRM接続設定は次のサイトを参考にすること
    - http://docs.ansible.com/ansible/intro_windows.html#windows-system-prep


- 管理対象マシン(インストール対象マシン)
  - Apache Launge版 Apache 2.4 がインストールされていること
  - PowerShell 5.0を利用できること
  - ファイアフォールが適切に設定されていること

## Role Variables

Role の変数値について説明します。

### Mandatory variables

以下の変数は必ず指定します。

- WinRM設定（group_varsもしくはhost_varsに設定）
  * ''ansible_user'': ユーザ名(string)
  * ''ansible_password'': パスワード(string)
  * ''ansible_port'': ポート(number)
  * ''ansible_connection'': 接続方式(string)
  * ''ansible_winrm_server_cert_validation'': 暗号化有無(string)

### Optional variables

以下の変数は任意で指定します。

- Apache サービス設定
  * ''VAR_Apache24_WIN_ServiceName'': Apacheサービス名 (string, default:"Apache2.4")
  * ''VAR_Apache24_WIN_Enable'': サービス自動起動有効・無効 (on|off, default: "off")
    + on : スタートアップの種類を[自動]に設定
    + off : スタートアップの種類を[手動]に設定
  * ''VAR_Apache24_WIN_Start'': 再起動有効・無効 (on|off, default: "off")
    + on : Apache サービスを再起動する。（停止している場合は起動）
    + off : Apache サービスの起動・停止操作を行わない。（停止している場合は停止のまま）


## Dependencies

特にありません。

## Usage

1. 本Roleを用いたPlaybookを作成します。
2. 必須変数を指定します。
3. 任意変数を必要に応じて指定します。
4. Playbookを実行します。


## Example Playbook

インストールとすべての設定をする場合は、提供した以下のRoleを"roles"ディレクトリに配置したうえで、
以下のようなPlaybookを作成してください。

- フォルダ構成
~~~
  - group_vars/
    ・ server1
    ・ server2
  - host_vars/
    ・ host1
    ・ host2
  - roles/
    ・ Apache_install/
    ・ Apache_setup/
  - Apache24_WIN_install.yml
  - Apache24_WIN_ossetup.yml
  - Apache24_WIN_setup.yml
  - conf.yml
  - hosts
  - site.yml
~~~

- マスターPlaybook サンプル「Apache24_WIN_ossetup.yml」
~~~
# Apache24_WIN_ossetup.yml
 - name: setup OS for Apache2.4
   hosts: all
   gather_facts: yes
   become: no
   tags:
    - os_setup
   roles:
     - Apache24_WIN_ossetup
~~~

## Running Playbook
- extra-vars を利用する場合の実行例
> ansible-playbook site.yml  -i hosts --extra-vars="@conf.yml"

- group_vars を利用する場合の実行例  
 group_vars で指定したグループ名が webserver1 の場合
> ansible-playbook site.yml  -i hosts -l webserver1

- host_vars を利用する場合の実行例  
host_vars で指定したグループ名が server1 の場合
> ansible-playbook site.yml  -i hosts -l server1

- 本Roleのみを実行する場合は、 --tags "install" を付け加える
> ansible-playbook site.yml  -i hosts --extra-vars="@conf.yml" --tags "os_setup"

# Copyright
Copyright (c) 2017 NEC Corporation

# Author Information
NEC Corporation
