Swift実践入門メモ
# 第３章 基本的な型

## import Foundation
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


### 上記の比較文が長ったらしく感じたので、短くしてみた①
```swift
let order2 = "abc".compare("aBc", options: String.CompareOptions.caseInsensitive)
order2 == ComparisonResult.orderedSame
```
### 上記の比較文が長ったらしく感じたので、短くしてみた②
```swift
"abc".compare("Abc", options: String.CompareOptions.caseInsensitive) == ComparisonResult.orderedSame
```

どちらもちゃんとtrueが返ってくる



## 3.5 optional型についての項目

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


## nilリテラルにはデフォルトの型はない！
```swift
let a = 5
let b = "あいう"

type(of: a) // Int.Type
type(of: b) // String.Type
```

数字や文字列はデフォルトでIntやStringといった型があてられるが、
nilの場合はそれがないので、しっかり型宣言しないとコンパイルエラーとなる。

```swift
let c = nil // エラー 'nil' requires a contextual typeというエラーがでる。
```

ではこれではどうか？

```swift
let c: Int = nil // エラー 'nil' cannot initialize specified type 'Int'
                 // エラー Add '?' to form the optional type 'Int?'
```
nilを許容するためにOpitional型を適用させなければならない。

```swift
let c: Int? = nil // nil
type(of: c) // Optional<Int>.Type
```

これでnilが入った'c'はOptional<Int>型として扱われるようになる。


## イニシャライザによる型の生成

まず、単なるString型からInt型へのイニシャライザを考えてみる。
```swift
let str = "123"    // "123"
type(of: str)      // String.Type

let int = Int(str) // 123
type(of: int)      // Optional<Int>.Type
```
イニシャライザされたintはオプショナル型として扱われている。
これは、数値への変換が不可能な文字列を渡された場合に、nilを返すためである。

```swift
let str = "1ab"       // "1ab"
type(of: str)         // String.Type

let ngInt = Int(str)  // nil
type(of: ngInt)       // Optional<Int>.Type
```

テキストの内容
```swift
let optionalInt = Optional(1)   // 1
print(type(of: optionalInt), String(describing: optionalInt))   // "Optional<Int> Optional(1)\n"

let optionalString = Optional("a")   // "a"
print(type(of: optionalString), String(describing: optionalString))   // "Optional<String> Optional("a")\n"
```

### 試してみた
この見本の一文をちょっと変えてみた。
```swift
let optionalString = Optional("a")   // "a"
print(optionalString)   // "Optional("a")\n"
```

しかし、上記print文に、`Expression implicitly coerced from 'String?' to 'Any'`という警告がでてしまった。
```
Expression implicitly coerced from 'String?' to 'Any'

FIX1   Provide a default value to avoid this warning
FIX2   Force-unwrap the value to avoid this warning
FIX3   Explicitly cast to 'Any' with 'as Any' to silence this warning
```

単に文字列"a"が出力されることを期待していたのだが、上記３つのどれかでFIXしなさいと言われている。
一つずつ試してみた。
```swift
print(optionalString ?? <#default value#>)   // FIX1の実行結果
print(optionalString!)                       // FIX2の実行結果
print(optionalString as Any)                 // FIX3の実行結果
```

よけいなことに首を突っ込んでしまった感があるので、とりあえず先に進むことにする。
基礎知識を入れ終わったあたりで深掘りしてみようと思う。


## アンラップ(値の取り出し)
Optional<Wrapped>型の値が持つWrapped型の値に対する操作を行うには、Optional<Wrapped>型の値の中からWrapped型の値を取り出す必要がある。

これを**アンラップ**という。

## アンラップの種類
### ①オプショナルバインディング
条件分岐や繰り返し文の条件にOptional<Wrapped>型の値を指定する。
if let文を用いて値が存在する場合のみ{}内の処理を実行するというやり方で値を取り出す。
nilの場合は処理をスキップすることにより、nilによるコンパイルエラーを安全に回避することができる。

```swift
let optionalA = Optional("a")

if let aStr = optionalA {
    print(type(of: aStr))
}
// String\n
```

### ②??演算子
値が存在しない場合にデフォルト値を設定することができる演算子。

```swift
let optionalInt: Int? = 1 // 1
let int = optionalInt ?? 3 // 1(値が存在なしない場合のみ3が代入される)
```

```swift
let optionalInt: Int? = nil // nil
let int = optionalInt ?? 3 // 3(値が存在しないので3が代入されている)
```
このように??演算子の後ろにデフォルト値を設定することでnilを回避することができ、安全に値を取り出すことができる。


### ③強制アンラップ
!演算子を使うことにより、強制的に値を取り出す処理をいう。
強制的に取り出すので、値が存在しない場合はエラーとなってしまう。

値が存在する場合↓

```swift
let a: Int? = 1 // 1
let b: Int? = 1 // 1

a! + b! // 2
```

2が出力される。

値が存在しない場合↓
```swift
let a: Int? = 1 // 1
let b: Int? = nil // nil

a! + b! // error
```
エラーとなる。
```
error: Execution was interrupted, reason: EXC_BAD_INSTRUCTION 
(code=EXC_I386_INVOP, subcode=0x0).
The process has been left at the point where it was interrupted, use "thread return -x" to return to the state before expression evaluation.
```

強制アンラップは実行時エラーが発生してしまう危険性やがあり、swiftの特徴でもある安全性を低下させるプログラムコードとなってしまうので、使用することは推奨されていない。
(値が存在しない場合はプログラムを落としたい時など、限定された状況のみの使用が想定される)
