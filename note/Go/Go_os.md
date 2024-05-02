## Exit
`defer`の内容も実行されない。
```go
func main() {
	defer fmt.Println("hello")
	os.Exit(1)
}
```