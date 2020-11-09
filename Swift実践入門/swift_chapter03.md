Swift実践入門メモ
## 第３章　基本的な型

### import Foundation
標準ライブラリでは出来ない操作を可能とするコアライブラリ。

rangeメソッド
```swift
"abcdef".range(of: "ab")
```
返り値は引数に指定している文字列の範囲を示す値
```swift
let options = String.CompareOptions.caseInsensitive
let order = "abc".compare("aBc", options: options)
order == ComparisonResult.orderedSame
```
2つの文字列の順序の比較
String.CompareOptions.caseInsensitiveは大文字小文字の区別をしないオプション。ドットでつなぐ。
optionsとして定義している。

compareメソッド。Appledeveloperにある通りStringクラスのインスタンスメソッドである。
https://developer.apple.com/documentation/foundation/nsstring/1414082-compare
文字列の比較をすることができる

順序含む文字列の一致をtrue/falseで返す。


#### 上記の比較文が長ったらしく感じたので、短くしてみた①
```swift
let order2 = "abc".compare("aBc", options: String.CompareOptions.caseInsensitive)
order == ComparisonResult.orderedSame
```
#### 上記の比較文が長ったらしく感じたので、短くしてみた②
```swift
"abc".compare("Abc", options: String.CompareOptions.caseInsensitive) == ComparisonResult.orderedSame
```

どちらもちゃんとtrueが返ってくる



### 3.5 optional型についての項目

// Optional型は列挙型として以下のように定義されている。
enum Optional<wrapped> {
    case none
    case some(wrapped)
}

// 型推論
let none = Optional<Int>.none
print(".none: \(String(describing: none))")

let some = Optional<Int>.some(1)
print(".some: \(String(describing: some))")

