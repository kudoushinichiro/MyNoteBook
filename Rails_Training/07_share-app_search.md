今回の課題では「検索機能」を実装した。
求められた仕様は以下の通り。

・検索時のパスは/posts/search
・フォームオブジェクト・ActiveModelを使い機能を実装する
・全ての投稿に対する検索とする（コメントは対象としない）

上記を満たす実装のために、実際に行なったことや、学んだことを振り返ってみる。

## ルーティングを設定する

```ruby
#routes.rb
  resources :posts do
    collection do
      get 'search'
    end
    resources :comments, shallow: true
  end
```
postsにネストする形でsearchを置いた。
これはRailsガイドの"コレクションルーティング"を参考にした。
https://railsguides.jp/routing.html#コレクションルーティングを追加する

collectionを用いたルーティングを行うことで、仕様にある/posts/searchというパスにすることができる。

## フォームオブジェクトとは
ここらへんの記事を読んで参考にしました。

https://tech.medpeer.co.jp/entry/2017/05/09/070758　(フォームオブジェクトを使ってみよう)
https://qiita.com/kumackey/items/4f8f0cd39d02299d74ed　(フォームオブジェクトを使って検索キーワードをcontrollerに送る)
https://qiita.com/9ro/items/10e3676741e09ffb98bb　(FormObjectを使ってほしい)

ちゃんと理解できているかは怪しいですが、DB(モデル)を介さずにActiveRecordを利用している時のような機能を得ることができる、ということと考えています。

これを適用するには、クラスの記載に以下を追記する必要があります。
```ruby
include ActiveModel::Model
```
こちらもRailsガイドを参考にしました。
https://railsguides.jp/active_model_basics.html#modelモジュール

## 検索機能を実装していく
実装の流れを考えてみる。
1. 検索のクラスを定義する(フォームオブジェクトを使うため)
2. 検索用のインスタンスを用意する
3. viewの検索フォームに入力された内容をコントローラーに渡す処理を書く
4. コントローラーは記述されている処理を実行して結果を返す（検索結果画面）
5. 検索のロジックを書く(searchメソッド)

#### ①検索のクラスを定義する
```ruby
# search_posts_form.rb
class SearchPostsForm
  include ActiveModel::Model
  include ActiveModel::Attributes
  # 属性を指定の型へ変換してくれる

  attribute :body, :string
  # 型の指定をしている。
  # キーがbodyのものはstring型となる。
end
```
とりあえず定義のみ。

#### ②検索用のインスタンスを用意する
```ruby
#application.rb
#関係ない部分は省略
  before_action :set_search_posts_form

  def set_search_posts_form
    @search_form = SearchPostsForm.new(search_post_params)
  end

  def search_post_params
    params.fetch(:q, {}).permit(:body)
  end
```
①で作ったクラスのインスタンスを"SearchPostsForm.new"で作っている。
検索フォームはヘッダーにあり、ヘッダーはapplication.html.slimからrenderされているため、現段階では全てのページを開くたびに`set_search_posts_form`が実行され、@search_formが生成されていることになる。

#### ③viewの検索フォーム
```html
# posts/_search_form.html.slim

= form_with model: search_form, url: search_posts_path, scope: :q, class: 'form-inline my-2 my-lg-0 mr-auto', method: :get, local: true do |f|
  = f.search_field :body, class: 'form-control mr-sm-2', placeholder: '本文'
  = f.submit 'Search', class: 'btn btn-outline-success my-2 my-sm-0'
```
"model: search_form"で格納するインスタンスの指定(renderされており、元々は@search_formを受け取っている)
"url: search_posts_path"で送信先をpostsコントローラーのsearchアクションにしている。


#### ④コントローラーの処理
```ruby
# posts_controller.rb

  def search
    @posts = @search_form.search.includes(:user).page(params[:page])
  end
```
定義されたsearchメソッド。
viewから受け取った@search_formに対してsearchメソッドを実行している。

#### ⑤検索のロジックを書く
①で定義したsearch_posts_formクラスにメソッドを追記していく。
```ruby
# search_posts_form.rb
　def search
    scope = Post.distinct
    scope = scope.body_contain(body) if body.present?
    scope
  end
```
このsearchメソッドの実行結果をコントローラーに返してあげる。
これで検索結果が入った@postsをviewに返してあげればよい。

個人的な好みの話なのですが、searchメソッド部分は
```ruby
def search
  scope = Post.distinct
  if body.present?
    scope = scope.body_contain(body)
  else
    scope
  end
end
```
の方がif文が取っ付きやすくて好きです。
ただ、記述量も多くなってしまうし、Rubyはコードをいかにシンプルに書けるか、といった所が特徴であり強みとも思うので、見本コードのように後置if文を使用するのが良いんだなぁと思いました。


こんな感じで今回は実装を行いました。

## おまけ
##### `:qについて`
ちょっと脱線した話なのですが、③のform_withの第3引数で記述されている`scope: :q`のqなのですが、これはいったい何処からやってきて命名されたものなのか分からず・・・
結構いろいろ記事など検索してみても、みなさん当たり前のように"q"をキーとしていて、ずっとモヤモヤしたまま実装を行なっていました汗

とある方の記事
![スクリーンショット 2020-09-14 17.27.19.png](https://tech-essentials-production.s3.ap-northeast-1.amazonaws.com/c186a2ec-635a-4fc5-a483-bf8d24a62ab2.png)
なぜ・・・

今回は使用していないのですが、ransackという検索機能のgemのドキュメントも目を通してみたのですが、
>The default param key for search params is now :q, instead of :search.
>
https://github.com/activerecord-hackery/ransack
とあり、キーは:qがデフォルトだよ。とは書いてたのですが、その由来までは言及されていない感じでした。（英語弱々なので見落としてるかも？）

よく分からないまま"q"とするのがとにかく気持ち悪かったので、とりあえず適当に"inputsearch"とキー名を付け直して実装をし、プルリクでだいそんさんに質問してみました。

結果・・・
##### `"q"はqueryのことだった！`
GETリクエストのパラメータをクエリと呼ぶのが一般的で、省略して"q"とするのが慣例のようですね。
全然オチのない話ですみません。みんな普通に知っていてもはや常識なことだったのだろうか・・・