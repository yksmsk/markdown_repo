## Virtual BoxとVagrant入れてみる

環境に合わせてVartual Box と、Vagrantのインストール資材を公式から入手

- [Virtual Box](https://www.virtualbox.org/ "Virtual Box")
- [Vagrant](https://www.vagrantup.com/ "Vagrant")

資材を入手したらぽちぽちして画面の指示に従い、インストールを完了させる。

VM用のディレクトリを作成する。
使用したいguestに合わせてディレクトリを作成しとくと作業が楽かも

Vagrantfileを作成する
​	` vagrant init —minimal hogehoge` [^1]  

[^1]: hogehoge:[Vagrant](https://app.vagrantup.com/boxes/search)が公開しているOSイメージの名前に置き換える

**hostとguestのCPUアーキテクトが異なる場合**

​	32bitマシン上で64bitのゲストOSを動かしたい、あるいはその逆の時は注意が必要
​	作成されたVagrantfileに修正を加える
​	※修正内容は明示的にアーキテクトを指定するだけ。実際にやってないので今回は割愛。

Vagrantfileができたら起動する
​	`vagrant up`

初回はOSイメージのダウンロードとVMの生成があるので時間がかかる

つらつらとコンソールに文字が流れていき、それが終わると起動が完了している。
​	`vagrant ssh`
でそのままSSHログインができる状態になっている