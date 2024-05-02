breakは必要ない。
以下のように複数のcaseをまとめられる。
```go
n := 5
switch n {
case 1,2:
	fmt.Println("1 or 2")
default:
	fmt.Println("unknown")
}
```
条件を記述することもできる。
```go
n := 5
switch {
case 4 < n && n < 6:
	fmt.Println("4 < n < 6")
default:
	fmt.Println("unknown")
}
```
ただし、最初の列挙型の方法と条件を記述する方法は1つのswitch内で混在させられない。やろうとするとエラーになる。

また、型によるswitchも行える。
```go
var x interface{} = "yeah"
switch x.(type) {
case int:
	fmt.Println("int")
case string:
	fmt.Println("string")
}
```
なお、型アサーションで渡す引数は`type`である必要がある。適当な`hoge`とかにするとエラーになる。