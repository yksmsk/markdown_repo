# SlackでSlash Commandsを作ってみたい

Slackのコマンドって使っていますか？？

弊社でも、社内のコミュニケーションツールとして、slackが導入されました。

事務的な連絡や、イベント告知等々使われてはおりますがそこまで活発に利用されている印象がまだありません。

そこで、slackの利用を加速するためにも、独自のslash commandsを作って見ようと思い立った次第です。



## やりたいこと

「コマンド打った時間を記録して出勤、退勤をつけたい」



もしかしたら既存であるかもしれないですが、今回はGoogleドライブ上のスプレッドシートに日付と出勤時間、退勤時間を追記し、月が変われば新規のファイルorシートを作成して追記、みたいなことをやってくれるコマンドを作ろうと思います。

コマンドがやるというより、「SlackからAPIをcallしてAPIにやらせる」が正しいです。



と、思いましたが、事前準備としてslack - APIの連携する大枠をまずは作ってちゃんと動くよね？

って部分を確認したいと思います。



てな訳で、今回の目標は

1. Slack側でコマンドを実行
2. リクエストを受けレスポンスを返す
3. Slackでレスポンス結果を表示

ぐらいのものを目指します。



とりあえずは引数に指定した文字越をオウム返しさせるぐらいのものを作ります。



## APIの準備

slash commandsではHTTPSな環境が求められるので、自前でサーバを建てるなり

どっかしら借りるなどして下さい。



今回はGoogleが提供してくれるGoogle Apps Script ([GAS](https://www.google.com/script/start/))をお借りします。**無料**です。



GASはJavaScriptをベースにしたサーバサイドなスクリプトです。

Googleアカウントがあればどなたでも利用可能です。



GASであれば、Googleドライブ上のスプレッドシートの操作もできちゃうので、やりたいことを実現できると思い採用しました。



- Googleアカウントを持っていなければ作成
- 上記URLにアクセス
- 「start scripting」を押下してProjectの新規作成
- 「新規スクリプト」を押下



```gas
function doPost(e) {
  var params = JSON.parse(e.postData.getDataAsString());
  var value = params.value;
  var now = getNow();  
  Logger.log(value);
  
  var response = {text : now};
  
  return ContentService.createTextOutput(JSON.stringify(response)).setMimeType(ContentService.MimeType.JSON);
}

function doGet(e) {
  var response = {now : getNow()}
  return ContentService.createTextOutput(JSON.stringify(response)).setMimeType(ContentService.MimeType.JSON);
}

function getNow() {
  var now = Utilities.formatDate(new Date(), "JST", "HH:mm");
  return now;
}
```

とりあえずサンプルを記載

Post用のメソッドとGet用のメソッドを用意

→最終的にはスプレッドシートに書き込みに行く処理を実装する



ブラウザからアクセスするとGet用のメソッドが動く

Postするときはculとかでアクセス

→slash commandsからのリクエストはPOST



## Slack側の設定

まずはこちらにアクセス：https://api.slack.com/apps

- Appを作成していなければ新規作成
- slash commandsを登録
  - コマンド
  - Usage
  - hint
  - RequestsURL：GASプロジェクト公開時のURL

あとはワークスペースへ公開するとコマンドが使えるようになるはず。







## まとめ

思いの外、簡単に連携ができましたね。

GAS側の処理はもう少し調べたらなんとかなりそう。



また別記事であげようと思います。



将来的にはwordpressと連携してローカルで書いたテキストファイルとかを渡してslash commandsでブログ記事アップとかできないかなと妄想。。。



どうですかね。