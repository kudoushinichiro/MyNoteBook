この課題を通じて学んだ点を述べていきます。


## Postモデルの作成について
今回はpostモデルを作成。前の課題で作成したUserモデルに続いて２つ目のモデルとなります。
Postモデル作成に当たっての注意や学びを以下に列挙

・**外部キーについて**
　
 foreign_keyはどのカラムにキー制約をつけるのか、という設定をすることが出来る。
今回だと、t.references :userすなわちuser_idについて外部キー指定する。
該当カラムには自由に値を入れることは出来ず、ユーザーテーブルに存在しているレコードのidしか設定できないという「制約」が与えられる。


・**アソシエーションの記述**

　今回のケースだと、postモデルの`belongs_to :user`、userモデルの`has_many :posts, dependent: :destroy`の記述がそれにあたる。
 それぞれのモデル（テーブル）関連づける場合は各モデルファイルに上記のようなアソシエーションの記述をする必要がある。この記載がないと、参照先(この場合はuser)テーブルにアクセスすることができない。
 尚、userモデルのdependent: :destroyの部分は、`userを削除したときアソシエーション先（この場合はposts）にもdestroyアクションを実行することができる（削除できる）`というものであり、この設定をしておかないと、userは削除されたが該当userの投稿は残ってしまうということになるので注意が必要（必ずしもその状態がNGという訳ではない？）
 
 
 ## carrierwaveについて
 課題２の一番大きな山でした。
 ポイントは複数画像の取り扱い方法についてという所。
 
複数枚場合だと、modelファイル等で記述を変える必要がありました。
 以下公式ページから引用
 
> **Multiple file uploads**
CarrierWave also has convenient support for multiple file upload fields.
  **ActiveRecord**
Add a column which can store an array. This could be an array column or a JSON column for example. Your choice depends on what your database supports. For example, create a migration like this:
**Note**: JSON datatype doesn't exists in SQLite adapter, that's why you can use a string datatype which will be serialized in model.
**Open your model file and mount the uploader:**
class User < ActiveRecord::Base
  mount_uploaders :avatars, AvatarUploader
  serialize :avatars, JSON # If you use SQLite, add this line.
end

という記載のとおり、`画像は配列形式でカラムに入れること`、`配列の中身はJSON型であること`と述べられており、複数枚仕様の場合は上記のような仕様で画像ファイルを取り扱う必要があるようです。

 ・**①配列形式での取り扱いについて**
 これは一つのカラムの中に複数の画像データを保存する必要があるため、配列でカラムに格納する必要があるということ。
 そのためコントローラーの記載を以下のように設定する。
 ```ruby
 def post_params
    params.require(:post).permit(:body, images: [])
  end
 ```
 `images: []`の記述でcreate時に配列形式でimagesを受け取ることが出来る。
 
 これで実際に投稿すると、テーブルにはこのように登録される。
 ```
 mysql> select * from posts;
+----+---------------------+-----------+---------+---------------------+---------------------+
| id | images              | body      | user_id | created_at          | updated_at          |
+----+---------------------+-----------+---------+---------------------+---------------------+
|  2 | ["sampleneko.jpeg"] | テスト    |       1 | 2020-08-16 21:34:03 | 2020-08-16 21:34:03 |
+----+---------------------+-----------+---------+---------------------+---------------------+
1 row in set (0.00 sec)
 ```
 
 上記は画像一つのみだが、["画像名"]としっかりと配列で保存されていることがわかる。仮に２つ画像を送信した場合は`["hoge.jpeg","fuga.jpeg"] `のような形となる。
 
 ・**②JSON型での取り扱いについて**
 まず前提として**imagesカラムをstring型**で設定しているケースを想定しています。その場合には以下の記述が必要となる。
 
 ```ruby
 class post < ActiveRecord::Base
  mount_uploaders :images, ImageUploader
  serialize :images, JSON
end
 ```
 
上記記述でstring型で扱われるはずのデータをJSON型として使用することが出来るようになる。
上記の設定を省いてしまうと、viewに正しくデータを渡すことが出来ず、画像を表示させることが出来ません。

ちなみにJSON型とは`JavaScript Object Notation`というデータフォーマットのことで、JavaScriptオブジェクトのように扱うことができるという特徴があるもののようです。

