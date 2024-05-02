よくある類の無名関数が書ける。
```go
f := func(x, y int) int {
	return x + y
}
i := f(1, 2)
fmt.Println(i)
// 出力：3
```
戻り値として関数を返すこともできる。
```go
func ReturnFunc() func() {
	return func() {
		fmt.Println("Hello")
	}
}

func main() {
	f := ReturnFunc()
	f()
}
```
関数を引数にとることもできる。
```go
func Call(f func()) {
	f()
}

func main() {
	Call(func() {
		fmt.Println("Hello")
	})
}
```
クロージャの実装は以下のようになる。
```go
func Later() func(string) string {
	var store string
	return func(next string) string {
		s := store
		store = next
		return s
	}
}

func main() {
	f := Later()
	fmt.Println(f("1"))
	fmt.Println(f("2"))
	fmt.Println(f("3"))
}
```
実行結果：
```
> go run main.go

1
2
```
最初は空文字が帰ってきている。
storeはreturnされる関数の外側の変数だが、関数があり続ける限り参照される。
このようなクロージャを利用することで関数が状態を持つことができ、余計なグローバル変数を宣言する必要がなくなるというメリットがある。

クロージャの応用として、以下のようなジェネレータを作れる。
```go
func integers() func() int {
	i := 0
	return func() int {
		i++
		return i
	}
}

func main() {
	ints := integers()
	fmt.Println(ints())
	fmt.Println(ints())
	// 出力：1、2

	otherInts := integers()
	fmt.Println(otherInts())
	fmt.Println(otherInts())
	fmt.Println(otherInts())
	// 出力：1、2、3
}
```
