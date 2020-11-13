本を読んでてよくわからなかったところ まとめ
学習を進めていれば勝手に解決するかもしれんが、忘れないようにメモ。

## compareの返り値って何よ問題
第３章で出会った疑問。

文字列の比較をするStringクラスのインスタンスメソッドを使用している箇所。

```swift
import Foundation

let options = String.CompareOptions.caseInsensitive
let order = "abc".compare("ABC", options: options)
order == ComparisonResult.orderedSame
```
最後の行でtrueが返ってきている。

っていうことは、`"abc".compare("ABC", options: options)`の返り値は
`ComparisonResult.orderedSame`になるという認識でいいのだろうか？
なんとかして確認したかったけど、出力の仕方がわからず・・・

`print(order)`だとエラーになってしまう。

### 期待していたもの
compareはStringのインスタンスメソッドで、勝手に勘違いしていてtrue/falseが返ってくると思っていた。
以下の場合、print(hoge)はtrueが出力されるものと考え、実行してみた。

```swift
import Foundation

var foo = "abc"
var hoge = foo.compare("abc")

print(hoge)
```

実際の結果・・・`NSComparisonResult`



#### 少し脱線
rubyからプログラミングの学習入ったからどうしても比較してしまうのだが、
rubyでいうStringクラスのインスタンスメソッドである`eql?`のようなものと思っていた。

```rb
puts "abc".eql?("abc")

# true
```

公式ページでメソッドを調べたら、`eql`とは全然違う模様。
というより、swiftには別でStringインスタンスメソッドで、`isEqual`というのが存在していた。
```swift
import Foundation
print("abc".isEqual("abc"))

// true
```

### ComparisonResultって？
なんか色々調べてみると`ComparisonResult`は列挙型で、３つのcase定数を持つということが分かった。
https://rusutikaa.github.io/docs/developer.apple.com/documentation/foundation/comparisonresult.html

ということはcompareメソッドはComparisonResultの持つcaseのどれかを返すということになると考えられる。

case orderedAscending ・・・ `<`
case orderedSame ・・・ `=`
case orderedDescending ・・・ `>`

今回は比較対象がイコールであったため、`orderedSame`という値を取得し、
返り値は`ComparisonResult.orderedSame`がとなった。

## compareの返り値
ということで冒頭のコードに戻ってみる。

```swift
import Foundation

let options = String.CompareOptions.caseInsensitive
let order = "abc".compare("ABC", options: options)
order == ComparisonResult.orderedSame

// true
```
2行目の`"abc".compare("ABC", options: options)`の返り値が`ComparisonResult.orderedSame`なのでtrueということとなる。

なんというかコードみれば一目瞭然なのだが、なぜこんなに気になってしまったのだろうか・・・

#### 返り値の確認方法はないのだろうか？

これは学習を進めれば何処かでデバックを学ぶことになるだろうから、そのうちわかるようになるんだろうけど。
上記コードの`let order`を定義した所で、中身を確認することは出来ないのだろうか？
`"abc".compare("ABC", options: options)`の返り値がパッと分かるような何かが・・・

例えば`print(order)`の出力が`ComparisonResult.orderedSame`となるのならコンソールですぐに確認できるのになぁ。

おしまい。
