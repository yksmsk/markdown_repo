# SlackでSlash Commandsを作ってみたい

Slackのコマンドって使っていますか？？

デフォルトでもいくつか用意されているらしいのですが、私は「/remind」ぐらいしか使ったことありません。



そこで、今回自分好みのコマンドを作って日々のちょっとしたことをSlackにやらせてみようと思います。



## やりたいこと

「コマンド打った時間を記録して出勤、退勤をつけたい」



もしかしたら既存であるかもしれないですが、今回はGoogleドライブ上のスプレッドシートに日付と出勤時間、退勤時間を追記し、月が変われば新規のファイルを作成して追記、みたいなことをやってくれるコマンドを作ろうと思います。



コマンドがやるというより、「SlackからAPIをcallしてAPIにやらせる」が正しいです。

ですので、今回のメインはSlackと連携するAPIの方です。

→Slackの自作Slash Commandsは調べるといくらでもやり方が出てきます



## APIの準備

slash commandsではHTTPSな環境が求められるので、自前でサーバを建てるなり

どっかしら借りるなどして下さい。



今回はGoogleが提供してくれるGoogle Apps Script ([GAS](https://www.google.com/script/start/))をお借りします。**無料**です。



GASはJavaScriptをベースにしたサーバサイドなスクリプトです。

Googleアカウントがあればどなたでも利用可能です。



GASであれば、Googleドライブ上のスプレッドシートの操作もできちゃうので、今回やりたいことを実現できると思い採用しました。



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

→実際にはスプレッドシートに書き込みに行く処理を実装する



ブラウザからアクセスするとGet用のメソッドが動く

Postするときはculとかでアクセス

→slash commandsからのリクエストはPOST

