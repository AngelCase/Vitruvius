`Stringer`インタフェースは`fmt`パッケージで定義されている。
```go
type Stringer interface {
    String() string
}
```
`String()`を実装することで、print時の出力形式を自由に決められる。
```go
type Greet struct {
	name string
}

func (g Greet) String() string {
	return fmt.Sprintf("I'm %v.", g.name)
}

func main() {
	greet := Greet{"Adam"}
	fmt.Println(greet)
	// 出力：I'm Adam.
}
```

## 参考
https://go.dev/tour/methods/17