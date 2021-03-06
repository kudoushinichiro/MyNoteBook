# 本実装にあたり苦戦した箇所の抜粋

## まずgitの使い方から
githubについては多少触ったことはあったが、しっかり理解していた訳でもなく、GUIツールを使用し何とか扱えていたというレベルだったので、ここでしっかり学習し直すことに。

紹介されていたUdemyのgitの講座を受講し、少し理解を深めることができた。
例えば `git checkout` など今まで聞いたこともなかったので学習して本当に良かったと感じています。ターミナルでgitを操作する恐怖も少し解消されました。（でもやっぱりGUIを使ってしまう）

#### git flow
```
実際の現場でよく使用される開発手法のこと。
ブランチの使用ルールに特徴がある。

・master リリース用のブランチ。開発者は一切手をふれない。
・develop 開発元となるブランチ。このブランチから開発者の作業ブランチを切る。
```

## 見本ブランチページを見る
という最初の1歩目でいきなりつまづきました。ローカルホスト画面でのエラー・・・

・yarnがありませんよ のエラー
 控えていないので細かな表示は覚えてませんが、yarnが無いことでのエラーでした。
  `そもそもyarnってなんだ？`というところから調べることに。
  
  #### yarnとは
 ``` node.jsのパッケージマネージャ。
  javascriptはクライアントサイドで動く言語だが、node.jsはサーバーサイドで使用できるjavascript。
  そのnode.jsのパッケージマネージャのひとつ。
  同じパッケージマネージャにnmpというのもあるが、そちらよりyarnの方が新しく、高性能ということである。（そうなの？）
  ```
 
 
 上記問題を解消したが続いてさらなるエラーが。
 
` ・Errno::ECONNREFUSED: Connection refused – connect(2)172.0.0.1.6379`
 
 というエラー表記が。
 エラー文全文をググってみすとローカルホストに接続できませんよ、という内容であることはわかったが、その原因について記載されている記事は見当たらず・・・
 エラー文ではなく、172.0.0.1.6379で調べるとどうやらredisであることが判明。
 `で、redisってなんぞや？`だったのでこちらも調べるところから。
 
 #### redisとは
```
インメモリ型のデータベース。webサーバーとは別に独自のサーバーを立ち上げることによって、そこにデータを保管することが可能になる。
 というもののよう。正直理解がかなり怪しいが、mysqlに保存するほどでもないデータを保存できる場所という認識でいる。あやしい。
 今回はそこにセッションデータを保管することに。
 ```
 
 
 redisを導入し、redisサーバーを起動したらローカルホストに無事接続できました。
 一つ気になっているのは`redis severって起動しっぱなしでいいの？`ということ。
 どうなんだろ。
 
 
 ## rubocopの指摘修正
 rubocopの指摘で一つ解決に時間がかかったものがありました。
 
` ・Rails/UniqueValidationWithoutIndex: Uniqueness validation should be with a unique index. validates :username, uniqueness: true, presence: true`
 
 これはモデルバリデーションでunique trueをつけているが、スキーマファイルにindexがついていませんよ　
 という指摘で、indexをつけておかないと、バリデーションを抜けて２重で保存できてしまう時がある（例えば同時に登録した場合など）
 
 なのでこれを解消するために、マイグレーションファイルでusernameにindexをつけるという変更をし、無事解消しました。
 
 ただ、indexを初期で設定されているemailの行に追加しようとするとマイグレーション時にエラーがでてしまうので、2行にわけて書くことに。
 
 ```ruby
 def change
    create_table :users do |t|
      t.string :email,            null: false
      t.string :crypted_password
      t.string :salt
      t.string :username, null: false

      t.timestamps                null: false
    end

    add_index :users, :email, unique: true
    add_index :users, :username, unique: true
 ```

  なんかすっきりしない感じはありますが、とりあえず問題は解消したのでOKということにしました。
  
  
  
  長くなってしまうのでこの辺で。
  振り返ってみると、環境構築にすごく苦労したなぁと思います。
  アプリケーションやサーバーなどの基本的な知識の不足も感じたので、併せて勉強していこうと思います。