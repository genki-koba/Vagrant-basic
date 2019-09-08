# vagrantでmysqlをインストール+mysqlの操作

参考サイト：
[centOS6.5にmysql5.6をインストールする](https://qiita.com/tiwu_official/items/5ff3fa38611de058704a)

[PHPからmysqlの操作](https://qiita.com/tiwu_official/items/3415fad87fb3a6d68666)

基本的には、これらのサイトに沿えばインストール、mysqlの実行ができるが、
環境や、インストールするmysqlのバージョン等によっては、エラーを吐き出すことも多々ある。

### エラー時に参考にしたサイト

*エラー*
```:ソケットに関するエラー(2002)
ERROR 2002 (HY000): Can't connect to local MySQL server through socket '/var/lib/mysql/mysql.sock' (2)
```
＜原因＞
今回は、そもそも/var/lib/mysql/mysql.sockが存在しないことが原因だった。

参考にしたサイト：
[[2002] MySQLのソケットエラー の原因と対処法](https://beyondjapan.com/blog/2016/03/2002-mysql-socket-error/)

### ホストOSとゲストOSにおける共有フォルダの同期

- Homesteadを用いたLaravelの環境構築などでは、ymlファイルの記述によって簡単に同期設定も行うことができるが、今回の場合は、自分で設定を行う必要がある。
以下の参考サイトを利用して、Vagrantの共有フォルダのリアルタイム同期を行う。

[Vagrantで共有フォルダの内容がリアルタイム同期されない件](https://qiita.com/sudachi808/items/edc304b3ee6c1436b0fd)

