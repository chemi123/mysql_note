# mysql_note
centos7でmysql8を使う際のメモ(インストールから)。  

```
// リポジトリに追加
$ yum localinstall  https://dev.mysql.com/get/mysql57-community-release-el7-11.noarch.rpm

// MariaDBの削除(インストールされているなら)
$ yum -y  remove  mariadb-libs

// 必要に応じて自動起動設定(大概は必要か)
$ systemctl enable mysqld

// 起動
$ systemctl start  mysqld

// 初期起動時に一時的なパスワードがログに吐かれるのでこれを使う
$ grep password /var/log/mysqld.log
.....[Note] A temporary password is generated for root@localhost: *****

// パスワードの変更を行う
// なっていないと思うが、"--skip-grant-tables"の設定になっていると変更できないので注意する
$ mysql_secure_installation
```

## 注
###--skip-grant-tables
認証(パスワード)なしでmysqldに接続できるようにする設定。当然といえば当然だが、alterやupdate等の更新はできない模様。
