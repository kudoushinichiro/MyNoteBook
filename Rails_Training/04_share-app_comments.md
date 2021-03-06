## コメント機能の実装について学んだこと、苦労したことなど

### コメント機能とは
ユーザーが投稿したPostに対してコメントをすることができる機能。
新たにcommentモデルを作成する必要がある。

既存モデルとのアソシエーションで注意する点は、
・一人のユーザーは複数のコメントをすることが出来る
・一つの投稿は複数のコメントを持つことが出来る

このような点に気をつけてアソシエーションを組むとよい。
Railsガイドの"Active Record の関連付け"という章を参考にした。

### ルーティングについて
コメントは投稿に対して行われることになるため、ネストしてあげる必要がある。
今回の課題では、shallowオプションを用いて実装する。
shallowオプションを使用してルーティングを書くことで、
**comment_idをparamsに持つアクション(show/edit/update/destroy)についてURLのposts部分を省くことができる。**(urlを短くしても一意性を確保出来ている)
URLをすっきりさせることができるということですね。

今回のルーティングのネストについては、Railsガイド"Railsのルーティング2.7"あたりを参考にした。ほぼ答え。

ネストについて
railsガイドから抜粋
https://railsguides.jp/routing.html#%E3%83%8D%E3%82%B9%E3%83%88%E3%81%97%E3%81%9F%E3%83%AA%E3%82%BD%E3%83%BC%E3%82%B9

>
2.7.2 「浅い」ネスト
前述したような深いネストを避けるひとつの方法として、コレクション (index/new/createのような、idを持たないアクション) だけを親のスコープの下で生成するという手法があります。このとき、メンバー (show/edit/update/destroyのような、idを必要とするアクション) をネストに含めないのがポイントです。これによりコレクションだけが階層化のメリットを受けられます。つまり、以下のように最小限の情報でリソースを一意に指定できるルーティングを作成するということです。

resources :articles do
  resources :comments, only: [:index, :new, :create]
end
resources :comments, only: [:show, :edit, :update, :destroy]
この方法は、ルーティングの記述を複雑にせず、かつ深いネストを作らないという絶妙なバランスを保っています。:shallowオプションを使うことで、上と同じ内容をさらに簡単に記述できます。

```ruby
resources :articles do
  resources :comments, shallow: true
end
```

### 非同期通信について
コメントを投稿・編集・削除を行う際、ページのリロードをせず画面の更新を行えるよう非同期通信機能を使用した。
非同期通信とはAjaxでリクエストを行いjavascript形式でレスポンスを受け取ることで、HTMLのファイルを読み込まず(ページ遷移を行わず)にページの一部分のみを更新することが出来る機能のことを言う。

今回はform_withの設定を`remote_true`とすることでajax通信を実装しています。
上記を設定することで、JSファイルがformの送信先として探されることになります。
なので、form_withをcreateで使用している場合であれば`comments/create.js.slim`ファイルが選ばれることになります。
※local: tureとするとHTML形式でのリクエストが行われて、`comments/create.html.slim`というファイルを探しに行ってしまう。

Ajaxでリクエストを受けたら、JS形式のファイルでレスポンスを返し、その内容がviewに反映されることになります。

レスポンスの例
```js
| $('変更をしたいHTML要素').prepend("追加したい文字列");
```

などのJSファイルを作成してあげればよい。
この内容が指定したDOMに差し込まれる形で更新される。

非同期通信についてはおそらく今後もかなり使う（ユーザビリティのためには必須と思う）ので、JavaScriptの知識も含めしっかり学習していきたい。
