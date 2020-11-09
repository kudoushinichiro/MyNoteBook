課題６の実装においては主に`中間テーブルを使用する際のアソシエーション`や`has_many関連付け`、`モデルメソッドの記述`について学んだ。


## relationshipモデルについて
フォロー・アンフォローを実装するためのモデル。
作成するカラムは、以下の通り。

```ruby
# マイグレーションファイル

t.references :follower, null: false, foreign_key: { to_table: :users }

t.references :followed, null: false, foreign_key: { to_table: :users }
```

この記述でどちらのカラムでもuser_idを扱うことができる。

ralationshipテーブルはいわゆる中間テーブルと言われるものとなっている。しかもカラムも同じくuser_idを外部キーとして指定しており、こういったケースではアソシエーションをしっかりと設定してあげる必要がある。

## アソシエーションについて
前述した通り、中間テーブルの際はアソシエーションの設定が少しややこしくなる。以下のような記述とした。

```ruby
# relationship.rb

class Relationship < ApplicationRecord
  belongs_to :follower, class_name: 'User'
  belongs_to :followed, class_name: 'User'
  

  validates :follower_id, presence: true
  validates :followed_id, presence: true

  validates :follower_id, uniqueness: { scope: :followed_id }
end
```

`class_name: 'User'`と補足設定することで、存在しないモデルを参照することを防いでいる。**要はどちらも"belongs_to :user"ということ。**

` validates :follower_id, uniqueness: { scope: :followed_id }`
この記述でfollower_idとfollowed_idの組をuniqueにした。１通りのみ許可のバリデーション。
uniquenessはRails Validationヘルパーの一つで属性の値が一意（unique）であり重複していないことを検証する。
（属性について同じ値のレコードがモデルにあるかをクエリを走らせ確認する）
   scopeオプションは他のカラムに一意性制約を求めることが出来る。
   今回のケースだと、ひとつの":follower_id"シンボルに紐づくscope先の"followed_id"カラムのレコードに対しuniquenessを実行している。


```ruby
# user.rb
# 途中は省略

①
has_many :active_relationships, class_name: 'Relationship', foreign_key: 'follower_id', dependent: :destroy
②
has_many :passive_relationships, class_name: 'Relationship', foreign_key: 'followed_id', dependent: :destroy


③has_many :following, through: :active_relationships, source: :followed
④has_many :followers, through: :passive_relationships, source: :follower
```

#### ①・②について
UserとRelationshipについてアソシエーションを組む必要がある。
簡潔に **"has_many :relationships"** としたい所だが、
relationshipテーブルに２つあるカラムはどちらもuserテーブルに紐づく中間テーブルとなっており、どちらのカラムとのアソシエーションなのかをしっかり区別してあげる必要がある。
よって、①と②２パターンのアソシエーションを記述するということになる。
①=>"User"とRelationshipテーブルの「follower_id」とのアソシエーションを設定している。active_relationshipsと命名しているため、class_nameでテーブル名を明示。かつforeign_key設定により参照するカラムも指定。

②=>"User"とRelationshipテーブルの「followed_id」とのアソシエーションを設定している。以下省略。

#### ③・④について
フォローしている/フォローされているuserを取得するためのもの。
ここの部分については、実装最中は「なぜこの記述が必要なのか？」ということが全然分からなかった。
なぜ？を深掘りし、以下のような結論に到達した。

=> **`ActiveRecordの関連付けのため。correctionメソッドを使用できるようにする。`**
これはRailsガイド4.3 has_many関連付けの詳細で解説されている。
https://railsguides.jp/association_basics.html#has-many関連付けの詳細


①②で設定した"〜〜〜_relationships"を経由し、sourceを持つuserを関連付ける。
user.followingを実行する→→"active_relationships"を経由する(userは自身のidを元にRelationテーブル/follower_idを参照)→→sourceオプションにより、idが一致したレコードのデータを取得できる
要は関連付けされたコレクションを取得できるよってこと。
  

## フォロー/アンフォローするメソッド
ここまででデータを保存する下準備はできたので、いよいよフォロー/アンフォローするためのロジックを書いていく。

```ruby
# user.rb

①
def follow(other_user)
  following << other_user
end

②
def unfollow(other_user)
  following.destroy(other_user)
end

③
def following?(other_user)
  following.include?(other_user)
end

④
def feed
  Post.where(user_id: following_ids << id)
end
```

主にhas_many関連付けをしたコレクションメソッド"following"を使用する。

"following"はUserクラスに設定されているインスタンスメソッドであり、"self"が省略されていることに注意！
この辺の細かな説明はクラス・インスタンスといったオブジェクト指向の理解が問われており、私はまだ理解に乏しいので割愛。

①follow
current_userが持つ"following"に、コントローラーより受け取った"other_userオブジェクト(@userのこと)"をフォローするたびに格納していくという処理。

<<はRubyのArrayクラスのインスタンスメソッド。
https://docs.ruby-lang.org/ja/latest/method/Array/i/=3c=3c.html
今回のケースだと、other_userオブジェクトをfollowingに破壊的に追加している。

破壊的に追加とは、レシーバであるオブジェクトそのものに変更を加えているということ。ちなみにレシーバ自体には変更を加えず処理を返すものは非破壊メソッドというようです。
（この記事がシンプルでわかりやすかった。https://chinatz.hatenablog.com/entry/2018/08/04/110201）

②unfollow
current_userが持つ"following"に格納されているother_user(コントローラーから受け取った@user)を削除する。

③following?
current_userが持つ"following"にview(_follow_area)より受け取った"userオブジェクト"が格納されているかどうかを確認する。
followingが、userと等しい要素を持つ時にtrue、持たない時にfalseを返す。

④feed
whereメソッドによりpostの中から、post.user_idが条件に一致するものを探す。
following_idsは、ActiveRecordの`collection_singular_idsメソッド`。Railsガイド4.3.1.7参照
following_idsメソッドに自分のuser.idを加えたものを戻り値として返すという処理を行なっている。


## その他
上記を記述すればあとはviewとコントローラーへの記述を残すのみです。
ここら辺は省略させて頂きます。
モデルにかなり多くの記述をしているので、1行づつ理解していかないと訳が分からなくなりそうですが、逆にコントローラーはスッキリしたのでよいのかな、と。
（Fat controllerはあまりよくないらしい）