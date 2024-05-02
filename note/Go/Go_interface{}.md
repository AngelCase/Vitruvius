any型のように使える。どんな型の変数でも代入できる。また、初期値としてnilを持つ。
```go
var x interface{}

x = 1
x = "A"
x = [3]int{1,2,3}
```
ただし、演算の対象として使うことはできない。
```go
x = 1
fmt.Println(x + 2) // エラー
```
型アサーションを使えば具体的な値を得られる。
```go
var i interface{} = "hello"

s := i.(string)
fmt.Println(s)
// 出力：Hello
```