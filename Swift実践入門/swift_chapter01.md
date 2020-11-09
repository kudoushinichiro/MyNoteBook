Swift実践入門メモ
## 第１章

### 実行環境について

#### REPLによる入力
REPLとは、'Read-Eval-Print-Loop'の略で
1行書くごとに実行されるインタラクティブな実行環境のこと

コマンドラインで１行づつ入力していく
swiftコマンドで作動
:exitコマンドで終了
:helpコマンドでコマンドリストの確認

ちょっとした確認などに使える

##### 試してみた

・変数に型が違うものを代入できないこと

```bush
$ swift
Welcome to Apple Swift version 5.0.1 (swiftlang-1001.0.82.4 clang-1001.0.46.4).
Type :help for assistance.
  1> var three = 3
three: Int = 3
  2> three = "san"
error: repl.swift:2:9: error: cannot assign value of type 'String' to type 'Int'
three = "san"
        ^~~~~

```

という感じで、一度INT型で定義された変数threeには文字列"san"は入らないことが確認できた。

#### 単一ファイルによる実行(.swiftファイル)

よくあるファイル単位によるプログラムの実行

テキストでは以下のように説明されている。

>hello.swiftというファイルに
>`print("Hello World")`
>と記述されていた場合、ターミナルで`$ swift hello.swift`と実行すると
>`Hello World`と出力される。


#### 複数ファイルによる実行(swiftパッケージマネージャー)
swiftパッケージマネージャーをinitすることで複数ファイルによる環境が構築される。
作成された環境は`swift run`コマンドで実行できる。

詳細は難しいので一旦スルーする。

