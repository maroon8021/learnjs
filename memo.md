# Memo of studying

## 役立ちそう
https://www.task-notes.com/entry/20141026/1414322858

## 181117 
### ./sspa server 動かない問題
**p.13** の `./sspa server` でサーバー立ち上げをwindowsでやろうとしたところちゃんと動かず。
まあ多分pythonとか入ってないんじゃないかなとか思った。
もちろん入っていないのは事実だったんだけど、 `cygwin` 入れろとあったのでそれで対応

入れて実行してみたが、改行がなぜかコードだと認識されていた。
http://tooljp.com/qa/bash-r-command-not-found-96A8.html
winのせいで改行コードが変わっているのだろうと思う、vscode上で変更

変更したら実行はできてそうだが今度pythonがない的なエラーでとまる
python入ってると思ってたら入ってなかった
説明はちゃんと読みましょう
https://ll.just4fun.biz/?Python/%E3%82%A4%E3%83%B3%E3%82%B9%E3%83%88%E3%83%BC%E3%83%AB/Cygwin%E7%92%B0%E5%A2%83%E3%81%ABPython%E3%82%92%E3%82%A4%E3%83%B3%E3%82%B9%E3%83%88%E3%83%BC%E3%83%AB

### AWS CLIをインストールしようとしたとき
**p.17**  `sudo easy_install pip` とやったらうまくいかなかった
そもそも `sudo` が動かない
それはつけなければ動いたがよかった、ただし下記エラーが出てきた
特に `pkg_resources.DistributionNotFound: The 'distribute==0.6.14' distribution was not found and is required by the application`
といったエラーが出てきた
そもそもdistribution的なやつもいないし、もちろんpipもいない
cygwin環境はwin環境とも少し違うようなので、検索するときはcygwinとつけると良さげ

`$ easy_install-2.7 pip `
でいけた・・・
http://d.hatena.ne.jp/yohei-a/20161211/1481414788

### s3バケットの作成
**p.21** 
S3バケットの作成、本来的な作り方を知りたいところではある
sspaがラップしてるだけ、ってことなら、AWS CLIでいけるのか？
-> CLIのコマンドがちゃんとあったので、それでやればok

`An error occurred (InvalidAccessKeyId) when calling the PutBucketWebsite operation: The AWS Access Key Id you provided does not exist in our records.`
エラーが出てたから何かと思ったら設定がイケてなかった。
変な空白が入っていた。
コピペ注意

その後
learnjs.benrady.comでやるとかぶってるからだめ、と言われる
learnjsSampleM.benrady.comとやると
`An error occurred (NoSuchBucket) when calling the PutBucketWebsite operation: The specified bucket does not exist`
と出る、若干理解できなかったがよく調べると大文字がだめだったみたい
https://dev.classmethod.jp/cloud/aws/us-east-s3-bucket-naming-rule-change-in-2018-march/


## 181118 
### vscodeでcygwinの読み込み
今度設定してみてもいいかも
https://blog.fujiu.jp/2016/07/visual-studio-code-cygwin.html
http://dynamicsoar.hatenablog.com/entry/2018/09/02/065456

https://vscode-doc-jp.github.io/docs/userguide/integrated-terminal.html

### memo of AWS
configとcredentialsは `home/user-name/.aws` にいる@windows

### user-select
Interesting
https://developer.mozilla.org/ja/docs/Web/CSS/user-select


## 181124
### アイデンティティプール
すべてのユーザーアイデンティティを保存するのに使えるコンテナ(cognito)

### なんかエラーでた
`aws: error: argument --identity-pool-id: expected one argument`
?? -> https://github.com/awslabs/aws-cognito-angular-quickstart/issues/133
https://github.com/danilop/LambdAuth/issues/51

### 公式サイト
https://developers.google.com/identity/sign-in/web/sign-in

## 181215
https://stackoverflow.com/questions/30425942/aws-cognito-invalid-identity-pool-configuration

冷静に内容見てみたらアイデンティティプールとかの設定ができてなかったように思える
そのため設定してみる


./sspa create_table conf/dynamodb/tables/learnjs/ learnjs
An error occurred (ResourceInUseException) when calling the CreateTable operation: Table already exists: learnjs
Waiting for table creation...Traceback (most recent call last):
  File "support/jsed.py", line 6, in <module>
    doc = json.load(json_file)
  File "/usr/lib/python2.7/json/__init__.py", line 291, in load
    **kw)
  File "/usr/lib/python2.7/json/__init__.py", line 339, in loads
    return _default_decoder.decode(s)
  File "/usr/lib/python2.7/json/decoder.py", line 364, in decode
    obj, end = self.raw_decode(s, idx=_w(s, 0).end())
  File "/usr/lib/python2.7/json/decoder.py", line 382, in raw_decode
    raise ValueError("No JSON object could be decoded")
ValueError: No JSON object could be decoded
.Traceback (most recent call last):
  File "support/jsed.py", line 6, in <module>
    doc = json.load(json_file)
  File "/usr/lib/python2.7/json/__init__.py", line 291, in load
    **kw)
  File "/usr/lib/python2.7/json/__init__.py", line 339, in loads
    return _default_decoder.decode(s)
  File "/usr/lib/python2.7/json/decoder.py", line 364, in decode
    obj, end = self.raw_decode(s, idx=_w(s, 0).end())
  File "/usr/lib/python2.7/json/decoder.py", line 382, in raw_decode
    raise ValueError("No JSON object could be decoded")

https://stackoverflow.com/questions/44564017/python-json-prase-valueerror-no-json-object-could-be-decoded
https://stackoverflow.com/questions/11174024/attributeerrorstr-object-has-no-attribute-read
https://qiita.com/Morio/items/7538a939cc441367070d
http://programming-study.com/technology/python-json-dumps/
http://programming-study.com/technology/python-file/

## 181223
結局上記のエラーはいい感じに解消できなかったので、普通のコマンドで対処するようにした。
`config.json` のあるディレクトリまでいって
`aws dynamodb create-table --cli-input-json file://config.json`
初回やったときは
`An error occurred (ResourceInUseException) when calling the CreateTable operation: Table already exists: learnjs`
って出てきた。
確かにちゃんと
`./sspa create_table conf/dynamodb/tables/learnjs/ learnjs`
が動いてなかっただけで、create-table自体は動いてた可能性があり、tableは作られていたよう。
なので
`aws dynamodb delete-table --table-name learnjs`
で消す
その後もう一度コマンド叩いたらちゃんと作られたぽい
AWSのダッシュボードには表示されていないんだけど、大丈夫なんだろうか。

role_policy.jsonが作られてなかったので、それは本を見ながら作成。
Resourceはもちろん自分でつくったtableの情報に置き換えた

とりあえず実装を進めてってリクエスト投げれるようになったら400エラーが出た
最初にでたのは `AccessDeniedException` だった
この辺のアクセスの詳細がわかっていないこともあったので、一旦 `AmazonDynamoDBFullAccess` を付与しておいた
https://stackoverflow.com/questions/34784804/aws-dynamodb-issue-user-is-not-authorized-to-perform-dynamodbputitem-on-resou

その後、 `ResourceNotFoundException` が出てきた
-> 直接的な原因かわからんけど、とりあえず一旦AWSコンソールの見てるリージョンが違うことがわかった
https://ap-northeast-1.console.aws.amazon.com/dynamodb/home?region=ap-northeast-1#
なぜcreate-tableとやっても反映されないかと思ったが、AWS consoleのリージョンが違った関係で見れていなかった