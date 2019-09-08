# vagrantでmysqlをインストール+mysqlの操作

### 参考サイト：

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

共有フォルダの同期ができていない場合、仮想マシン起動時(vagrant upを叩いた時)に以下のようなメッセージが表示される。

```
Vagrant was unable to mount VirtualBox shared folders. This is usually
because the filesystem "vboxsf" is not available. This filesystem is
made available via the VirtualBox Guest Additions and kernel module.
Please verify that these guest additions are properly installed in the
guest. This is not a bug in Vagrant and is usually caused by a faulty
Vagrant box. For context, the command attempted was:

mount -t vboxsf -o uid=1000,gid=1000 var_etc_html_public /var/etc/html/public

The error output from the command was:

mount: unknown filesystem type 'vboxsf'

```
訳すと以下のようなメッセージになる。
```
VagrantはVirtualBoxの共有フォルダをマウント（使用可能に）することができませんでした。
これは大抵の場合、vboxsfというファイルシステムが利用できないことに起因します。
このファイルシステムは、VirtualBox Guest Additionsとカーネルモジュールを利用して使用可能にします。
ゲストOS上で、Guest Additionが適切にインストールされているか確認してください。
これは、Vagrantにおけるバグであり、大抵の場合、VagrantBoxの欠陥によって引き起こされます。
状況を説明すると、コマンドが（VagrantBoxの中で）試みられ、

mount -t vboxsf -o uid=1000,gid=1000 var_etc_html_public /var/etc/html/public

上記のコマンドにより、以下のエラーが発生しました。
mount: unknown filesystem type 'vboxsf'
```

一般的にはマウントエラーと言われているそう。
エラー文を理解できれば、とてもシンプルな答えが見つかります。

まず、このようなメッセージすら表示されない場合は、VagrantBoxへの記述がなかったということなので、
自分でコマンドを利用して実行する必要がある。
[Virtualboxの共有フォルダ設定](https://qiita.com/haseken/items/982c5369988636991a4a)

今回は、
```
mount: unknown filesystem type 'vboxsf'

- vboxsfを利用してマウントしようとしたコマンド（これはcentos/7のvagrant box追加時に前もって入力されていたコマンド）が存在したが、vboxsfというファイルシステムのタイプを認識できなかった。
```

というメッセージである。

VirtualBoxにはもともとGuest Additionがインストールされているが、Guest Additionが適切されているか確認せよとのメッセージなので、バージョンの違いが原因だったということです。

- Homesteadを用いたLaravelの環境構築などでは、ymlファイルの記述によって簡単に同期設定も行うことができるが、今回の場合は、自分で設定を行う必要がある。
以下の参考サイトを利用して、Vagrantの共有フォルダのリアルタイム同期を行う。

参考サイト：

[Vagrantで共有フォルダの内容がリアルタイム同期されない件](https://qiita.com/sudachi808/items/edc304b3ee6c1436b0fd)

[Vagrantでマウントエラーが発生したときの解消方法](https://qiita.com/chubura/items/4166585cf3f44e33271d)

