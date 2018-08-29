# mysql_note
centos7でmysql8を使う際のメモ(インストールから)。  

```
// リポジトリに追加
$ sudo yum localinstall  https://dev.mysql.com/get/mysql57-community-release-el7-11.noarch.rpm

// MariaDBの削除(インストールされているなら)
$ sudo yum -y  remove  mariadb-libs

// 必要に応じて自動起動設定(大概は必要か)
$ sudo systemctl enable mysqld

// 起動
$ sudo systemctl start  mysqld

// 初期起動時に一時的なパスワードがログに吐かれるのでこれを使う
$ grep password /var/log/mysqld.log
.....[Note] A temporary password is generated for root@localhost: *****

// パスワードの変更を行う
// なっていないと思うが、"--skip-grant-tables"の設定になっていると変更できないので注意する
$ mysql_secure_installation

// 再起動
$ sudo systemctl restart mysqld

// rootで接続。変更したパスワードが反映されていることを確認する
$ mysql -u root -p
```

## 注意事項


## 設定メモ
### default_authentication_plugin
mysqldに接続する際の認証プラグイン。  
MYSQL5.7まではmysql_native_passwordが使われていたが、v8からデフォルトでcaching_sha2_passwordに変更された。  
注意事項としてはclientで対応してなかったりするので(どの程度対応されているのかまだ良くわかってない)、その場合はユーザのパスワードにmysql_native_passwordをセットしてやる必要がある。  
```
// 例
> ALTER USER 'user'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password'
```

### skip-grant-tables
認証(パスワード)なしでmysqldに接続できるようにする設定。当然といえば当然だが、alterやupdate等の更新はできない模様。
