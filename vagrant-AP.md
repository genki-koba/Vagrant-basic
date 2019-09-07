# VagrantでApacheとPHPの環境構築

### 以下を参考に構築

[vagrantを用いたPHPの環境構築](https://qiita.com/tiwu_official/items/f135e6b6fbbe3ec6aa54)

[Vagrantで仮想マシンに接続＆Webサーバを立ち上げてみよう](http://vdeep.net/vagrant-start-web-server)

### Vagratfileで指定したIPアドレスなのに、接続が拒否される場合
- ssh接続後、以下のコマンドでファイアフォールをオフにして解決するパターンが多いそう
```
sudo systemctl stop firewalld
```
参考URL：[VagrantでCentOS7を立てたが、httpアクセスが繋がらない時にやったこと](https://qiita.com/ta__ho/items/1bdd8403a15be7411e20)

- また、CGIプログラムを動かすには、当然Apacheが起動している必要がある
```
sudo service httpd start
```
- 以下のコマンドで、仮想マシンの起動時にサーバー（Apache）を自動で起動するように設定できる
```
sudo chkconfig httpd on
```

#### GUIからの接続時などで接続できない場合、以下のコマンドで、ssh接続設定を確認する
```
vagrant ssh-config
```

参考URL：[Vagrantで作成したローカルサーバにFTPクライアント(Cyberduck)からログインできないときの解決法](https://qiita.com/noraworld/items/a0fac559d2d6e76d50f8)


### また、実際の開発時には、ディレクトリやファイルの権限を付与することが必須になる。

[rootユーザーとは](https://eng-entrance.com/linux-root)
