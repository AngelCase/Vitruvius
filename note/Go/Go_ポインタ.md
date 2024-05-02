ポインタはメモリ上のアドレスと型情報を保持する。
データ構造を間接的に参照、操作できる。
```go
// 値渡し
func Double(n int) {
	n *= 2
}

// 参照渡し
func DoubleV2(n *int) {
	*n *= 2
}

func main() {
	var n int = 100

	Double(n)
	fmt.Println(n)
	// 出力：100

	DoubleV2(&n)
	fmt.Println(n)
	// 出力：200
}
```
主に値型の操作に使われ、参照型は元からその機能を含んでいる為、基本的には不要。
