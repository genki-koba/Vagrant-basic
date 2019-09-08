# vagrantでmysqlをインストール

参考サイト：
[centOS6.5にmysql5.6をインストールする](https://qiita.com/tiwu_official/items/5ff3fa38611de058704a)

基本的には、このサイトに沿えばインストールできるが、環境や、インストールするmysqlのバージョン等によっては、エラーを吐き出すことも多々ある。

#### エラー時に参考にしたサイト

*エラー*
```:ソケットに関するエラー(2002)
ERROR 2002 (HY000): Can't connect to local MySQL server through socket '/var/lib/mysql/mysql.sock' (2)
```
＜原因＞
今回は、そもそも/var/lib/mysql/mysql.sockが存在しないことが原因だった。

参考にしたサイト：
[[2002] MySQLのソケットエラー の原因と対処法](https://beyondjapan.com/blog/2016/03/2002-mysql-socket-error/)
