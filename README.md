# SpzDocker
## 概要
2018-11-9実施予定のサポータズColabの勉強会に使うリポジトリです。
VagrantとVirtualBox, CoreOSで実装した開発環境となります
当日は、こちらのリポジトリをあらかじめCloneしておいてください

## 前提
下記、VirtualBoxとVagrantをそれぞれインストールしておいてください  
VirtualBoxのインストール: http://www.oracle.com/technetwork/server-storage/virtualbox/downloads/index.html?ssSourceSiteId=otnjp  
Vagrantのインストール: https://www.vagrantup.com/  

## 使い方
1. VMの起動
`vagrant up`でCoreOSを使った開発環境をVirtualBox上に作成します
同時に、勉強会で使うrubyがインストールされたDockerImageもpullしてきます。

実際に会場で行うとpullに時間がかかる可能性があるため、できればあらかじめ実行しておいてください

`vagrant up`実行時にNFSでディレクトリ共有を行うため、ログインパスワードが聞かれますので、その際はホストマシンにログインしているユーザのパスワードを入力してください

2. VMの終了
`vagrant halt`でVMを終了します。使わない場合は、終了しておきましょう


2018/11/9 追記
一部の方で、起動できないということがありました。

https://github.com/Hare8563/SpzDocker/issues/2

その場合は、こちらのコメントを参考に、VagrantFileを修正して実行してみてください
