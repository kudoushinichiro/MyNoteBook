## クラスと構造体

## 値渡しと参照渡し

### 参照渡し(クラス)

インスタンスが値への参照を表す型
参照型の値の代入は、インスタンスに対する参照の代入を意味する

```swift
import Foundation

class IntBox {
    var value: Int

    init(value: Int) {
        self.value = value
    }
}

var a = IntBox(value: 1)
var b = a

a.value // 1
b.value // 1

a.value = 2

a.value // 2
b.value // 2 bはaと同じインスタンスを参照している。
```

### 値渡し(構造体、列挙型)

インスタンスが値への参照ではなく、`値そのもの`を表す型
値型の値の代入は、インスタンスが表す値そのものの代入を意味する
受け渡しのたびにコピーされている

```swift
struct Color {
    var red: Int
    var green: Int
    var blue: Int
}

var a = Color(red: 255, green: 0, blue: 0) // aに赤を代入
var b = a // bに赤を代入
a.red = 0 // aに黒に変更する

a.red // 0
a.green // 0
a.blue // 0
// aは黒になる

b.red // 255
b.green // 0
b.blue // 0
// bは赤のまま
```
bはaが持つred: 255, green: 0, blue: 0を`値`として代入されている




##  構造体とは
### ストアドプロパティの変更について
・ストアドプロパティの組み合わせで１つの値を表す
・構造体のストアドプロパティの変更は再代入が必要
・なので定数のストアドプロパティは変更できない

```swift
import Foundation

struct SomeStruct {
    var id: Int
    
    init(id: Int) {
        self.id = id
    }
}

var variable = SomeStruct(id: 1)
variable.id = 2 //ok

let constant = SomeStruct(id: 1)
constant.id = 2 // コンパイルエラー

```
`Cannot assign to property: 'constant' is a 'let' constant`

#### メソッド内でストアドプロパティの変更をする場合

構造体のストアドプロパティの変更を伴うメソッドにはmutatingキーワードが必要

```swift
import Foundation

struct SomeStruct {
    var id: Int
    
    init(id: Int) {
        self.id = id
    }
    
    mutating func someMethod() {
        id = 4
    }
    
}

var variable = SomeStruct(id: 1)
variable.someMethod()
variable.id // 4
```
mutatingをつけないとエラーが出る
`Cannot assign to property: 'self' is immutable`
