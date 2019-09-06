# Vagrant基礎（Vagrantへの接続まで）

＜実行環境＞
OS：macOS Sierra 10.13.4
VirtualBox: 5.2.20
Vagrant: 1.9.1

### 事前準備（ディレクトリの作成）
#### 1. フォルダを作成
```
mkdir /Users/ユーザー名/MyVagrant
```
- ディレクトリの作成はどこでも良い。
- 今回は、ホームディレクトリからアクセスしやすい場所に配置している。
（MyVagrantは任意のディレクトリ名）
#### 2. MyCentOSへ移動
```
cd /Users/ユーザー名/MyVagrant
```
___
## まず、Vagrant Boxとは？
仮想マシンのイメージ。
仮想マシンを作成するには、様々な設定が必要であり、精通した技術者でないとこの設定の難易度は非常に高い。
Web上（[Discover Vagrant Boxes - Vagrant Cloud](https://app.vagrantup.com/boxes/search)）ではこの設定がすべて記述されたファイルが無料配布されているため、これを利用して仮想マシンの作成を行う。
(一昔前は、専門職のインフラエンジニアでないとローカルサーバーの構築すら難しすぎてできなかったという、、、便利な時代に生まれて良かった)
___

#### ステップ１ 〜vagrant boxを追加する〜
上記のように、あらかじめ設定が記述された仮想マシンイメージのクローンをまず行う。
下記のようにボックスを追加することによって、
Users/ユーザー名/.vagrant.d/boxes/
にvagrant boxがダウンロードされる。
この.vagrant.dは、他の仮想マシン専用ディレクトリ（今回で言うステップ２のディレクトリA）での設定も追加されていくため、これを削除してしまうとすべての設定ファイルが消えてしまう。
```
vagrant box add centos/7
```

___
#### ステップ２ 〜vagrantを初期化する〜
続いて、
任意のディレクトリAを作成し、そのディレクトリAに移動。下記コマンドで、Vagrantfileを作成する。
```
vagrant init centos/7
```
（**ステップ１でクローンしたvagrant boxを元にVagrantfileを作成している=vagrantを初期化している=ここではじめて仮想マシンを作成している**）
___

#### ステップ３ 〜vagrantを起動する〜
```
vagrant up
```
このコマンドは、bring vagrant upの意味。(bring upはITにおいて、**最初のプロセス（オペレーティングシステム）をロードして起動させる。という定義がある。**)

<br>

##### *以下のようなエラーメッセージが表示された場合の対応
```
OpenSSL SSL_read: SSL_ERROR_SYSCALL, errno 54
```
（原因）ダウンロード一時ファイルが残っている
```
rm ~/.vagrant.d/tmp/*
```
と実行してから、vagrant upをやり直す。
___

#### ステップ４ 〜SSH鍵ファイルの作成〜

SSH とは Secure Shell（セキュアシェル）の略で、暗号や認証の技術を利用して、安全にリモートコンピュータと通信するためのプロトコル。
ホストOSとゲストOSとのやり取りは SSH で行う。
SSH でやり取りするのに必要な鍵を作成する。

まずは、既にSSH鍵ファイルが作られていないか確認します。
ホームディレクトリ移動します。
```:ターミナル（ホームディレクトリに移動するコマンド）
cd ~/
```
ホームディレクトリに移動した後に、下記コマンドを打ち、SSH鍵ファイルが存在しないか確認します。
```:ターミナル（SSH鍵ファイルが存在するか確認するコマンド）
ls -la I grep .ssh
```
id_rsa と id_rsa.pub が表示されれば、既にSSH鍵ファイルはあります。

無い場合には、作成します。

```:ターミナル（SSH鍵ファイルを作成するコマンド）
ssh-keygen -t rsa
```
Enter file in which to save the key と保存するディレクトリを聞かれるので、そのまま enter を押します。

Enter passphrase パスフレーズを求められますので、任意のパスフレーズを入力してください。
再度パスフレーズを求められるので、先程入力したパスフレーズを入力してください。
これでSSH鍵ファイルが作成されます。

確認のために先程のコマンドを入力してみましょう。
```:ターミナル（SSH鍵ファイルが存在するか確認するコマンド）
ls -la I grep .ssh
```
id_rsa と id_rsa.pub が表示されます。
___

#### ステップ５
ゲストOSにssh接続する（windowsではsshコマンドは使えないらしい）
```
vagrant ssh
```
___
### ＊POINT＊

以上のステップをまとめると以下のようになる。
Vagrant boxを追加し、
vagrant initで初期化、
vagrant upで起動。

ただし、他の方法もあり、これも多用される。（むしろ複数人での開発時は、こっちがメイン）

最初のステップでVagrantfileを作成し、
その中で、boxの追加をスクリプトで記述する方法。

以下のように、Vagrantfileに事前にすべての設定を記述しておくことにより、開発環境構築における人為的ミスを防ぐことができる。
```:Vagrantfile
Vagrant.configure("2") do |config|
  config.vm.box = "centos/7"
  config.vm.provision = :shell, path: "bootstrap.sh"
  config.vm.network = :forwarded_port, guest: 80, host:8080
  # config.vm.network "private_network", ip: "192.168.33.10", virtualbox__intnet: "mynetwork"
end
```
- config.vm.boxでboxを指定。
- config.vm.provisionでvagrant起動時の初期化処理を指定。
- config.vm.networkでポート80を8080に割り当てる。
- config.vm.hostname等でホスト名を指定しても良い。
他にも様々な設定がある。

config.vm.provisionについて：
*プロビジョニングとは、必要に応じてネットワークやコンピューターの設備などのリソースを提供できるよう予測し、準備しておくこと。

### ー ディレクトリ構造 ー

今回用いたディレクトリ構造
```
/Users
    └ ユーザー名
         ├ MyVagrant
         |    └ MyCentOS
         |        ├ Vagrantfile
         |        ├ .vagrant 
         ├.vagrant.d
```
尚、
- .vagrant
- .vagrant.d
に関してはそれぞれ、
- vagrant box
- vagrant init
コマンドで自動生成されるファイル
___

#### 参考になるサイト

[vagrantを用いたPHPの環境構築 - Qiita](https://qiita.com/tiwu_official/items/f135e6b6fbbe3ec6aa54)
[Vagrant 事始め 04 - Vagrant のネットワーク設定 - Qiita](https://qiita.com/centipede/items/64e8f7360d2086f4764f)
<https://qiita.com/ftakao2007/items/0ec05c2ef3c14cdbea11>
[Vagrant入門 (全13回) - プログラミングならドットインストール](https://dotinstall.com/lessons/basic_vagrant)
