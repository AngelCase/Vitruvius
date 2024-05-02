引数の型を`...`とすることで可変長引数を指定できる。
この引数はスライスとして渡される。
```go
func sum(s ...int) int {
	n := 0
	for _, v := range s {
		n += v
	}
	return n
}

func main() {
	fmt.Println(sum(1, 2, 3))

	slice := []int{1, 2, 3, 4, 5}
	fmt.Println(sum(slice...))

}
```