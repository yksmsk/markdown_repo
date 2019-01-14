# React Native で遊ぼう

![React Native](https://camo.qiitausercontent.com/8f4d0015c77a3dd3afc74d65f59e36ec51343137/68747470733a2f2f71696974612d696d6167652d73746f72652e73332e616d617a6f6e6177732e636f6d2f302f33303434302f30363231663936662d616462642d316364622d636466362d6234636264303437633336642e706e67)
巷で噂の[React](https://reactjs.org/)をモバイルで使えるようにしたJavaScriptのライブラリ。

+ どうやれば使えるの？
+ そもそも何ができるの？

っていう事始め的な部分をまとめてみます。



## Reactとは

そも、Reactって？

**「FacebookがOSSとして公開しているJSのライブラリ」**です。 

画面の上の部品(UI)をお手軽に作れるようになるよーってことです。

Facebookが公開しているだけあって、Facebookの画面はReactを使って作られています。

Instagramも同様。



## Hello world

[こちら](https://qiita.com/rgbkids/items/8ec309d1bf5e203d2b19#%E4%B8%80%E7%95%AA%E7%B0%A1%E5%8D%98%E3%81%AA%E3%82%B5%E3%83%B3%E3%83%97%E3%83%AB%E3%82%B3%E3%83%BC%E3%83%89)からお借りしました。

```HTML
<!DOCTYPE html>
<html>
  <head>
    <script src="http://fb.me/react-0.13.3.js"></script>
    <script src="http://fb.me/JSXTransformer-0.13.3.js"></script>
  </head>
  <body>
    <div id="app"></div><!-- ←ここに埋め込まれる -->

    <!-- 以下、Reactのプログラム -->
    <!-- (jsxを使っているのでtext/jsxと書く) -->
    <script type="text/jsx">
     var HelloWorld = React.createClass({
       // <HelloWorld />をレンダリング（表示）
       render: function() {
         return (
           <p>Hello!World!</p>
         );
       }
     });

     // id='app'に<HelloWorld />を埋め込む（マウント）
     var m = React.render(<HelloWorld />, document.getElementById('app'));
    </script>
  </body>
</html>
```

インストール等を省くためにFacebookのReact.jsをscriptタグで指定しています。

Reactはボタンやテキストを描画し、即座に画面に反映させるような構造の画面に強いと言われています。

ちゃんとお勉強したい人は[公式のチュートリアル](https://reactjs.org/tutorial/tutorial.html)があるので一度やってみるといいと思います。



## 本題

### React Nativeって？

Reactをモバイル向けに使えるようにしたもの。同様にFacebookが公開してくれてます。

React Nativeを使うことによってiOSとAndroidのネイティブに描画されるアプリを作成できる。



### ネイティブって？

クロスプラットフォーム開発でよくあるのがWebViewベースのアプリ。

トドのつまり、Webアプリを表示しているだけのアプリがあります。

方式としては利点もあるが制限も多く、何よりレスポンスがイマイチとなっている。。。

その点、React Nativeで作成した場合は各プラットフォームのネイティブなUIを使用し、各UIのスレッドとは別に動作するためパフォーマンスも高く維持できる利点がある。

(もちろん、Reactの知識が必要になるので学習コストはそこそこかかる問題はある。)

逆に言えば、WebでReactを使っている人であれば、そこまで苦にせずモバイルアプリの開発ができてしまうということでもある。

### 実際に導入してみた

サンプルアプリを動かすところまでやってみる。

の前に。

React Nativeのプロジェクトを作成するには２通りの方法がある。

1. create-react-native-app

2. React Native CLI



create-react-native-appだとExpoというアプリを通じてビルド不要のデバッグを行えます。

→公式で推奨されている手法がこちら



パッケージマネージャをインストール

今回はせっかくなので [Yarn](https://yarnpkg.com/ja/)を使用する。

何者？

Facebookが公開しているnode.jsと互換性のあるJavaScriptパッケージマネージャです。

- Node.jsを用いて導入する場合は以下を実行

`npm install -g yarn`

- Homebrew(MacOS用のパッケージマネージャ)を用いて導入する場合は以下を実行

`brew install yarn`

※コマンドの実行について、管理者権限が必要であれば`sudo`を先頭につける(以降も同様)

- React Nativeの導入

`yarn global add create-react-native-app`

- 開発モードで起動するためのツール「watchman」をHomebrewで導入

`brew install watchman`



- サンプルプロジェクトの作成

`create-react-native-app SampleProject`

- プロジェクト起動

```
cd SampleProject
yarn start
```



コンソールを眺めているとQRコードが表示されおもむろにブラウザが立ち上がる

→開発ツールっぽい

→ログとか、コマンドラインでやる操作をGUIでできる

コンソールに表示されている内容としてはアプリの起動方法を選んでねって感じ

| 開発ツール                                                   | コンソール |
| ------------------------------------------------------------ | ---------- |
| ![devtool](https://github.com/yksmsk/markdown_repo/blob/master/pic/devtool.png?raw=true) | ![console](https://github.com/yksmsk/markdown_repo/blob/master/pic/console.png?raw=true) |



サンプルアプリの動作確認

- Expoを導入した実機(LAN内)
- XcodeのiOS Simulator
- AndroidStudioのAVD

実機での確認が一番楽チンかなと思います(動作未確認)

専用のアプリをストアからインストールし、表示されるQRコードを読み込むだけ



今回はiOSのシミュレータで起動

画面の指示に従ってiOS Simulatorを起動・・・するも

→シミュレータは立ち上がるがアプリが起動せず

　→そもシミュレータにインストールされない。。。



よくわからーん



### React Native CLIを試してみる。

`yarn global add react-native-cli`

- サンプルアプリを作成

`react-native init SampleApp`

- iOSで起動

`react-native run-ios`



こちらだと問題なくシミュレータが起動し、しばらくするとアプリのインストールが始まった。

→だいたい5分くらいかかる

|   変更前   |   変更後   |
| ---- | ---- |
| ![ios1](https://github.com/yksmsk/markdown_repo/blob/master/pic/ios-simulator.png?raw=true) | ![ios2](https://github.com/yksmsk/markdown_repo/blob/master/pic/ios-simulator2.png?raw=true) |


サンプルのコードを書き換えると画面に表示される文言が書き換わるのが確認できる。

→あくまでもJavaScriptの変更なのでアプリ自体は再ビルド不要(画面の更新のみ)



## まとめ

React Nativeを使ったサンプルアプリを動作させるところまでご紹介しました。

モバイル系の開発は基本クロスプラットフォームでやることが多いのかなーと思うので、1つでも手法を知っていることは強みになるかと思います。

また、Reactはフロントエンド側でも新しい技術なので、身に付けておくとナウいエンジニアっぽく見られると思います。

実際に触ったことがなくても知識としてどういうものなのか知っているだけでも話のネタになる印象。

yarnも2016年にでたばかりのものなので、npm派の人も一度は触ってみると良いかと。

新しい技術も臆せずどんどん取り入れていきましょう。



社内開発でアプリ作る機会があればぜひ試してみたい。みてもらいたい。

→Gr施策で作ろうとはしてます。。。
