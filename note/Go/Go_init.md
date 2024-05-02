`init()`はパッケージの初期化に利用される特殊な関数で、以下の例だと`main()`より先に実行される。
```go
func init() {
	fmt.Println("init")
}

func main() {
	fmt.Println("main")
}
```
出力：
```
init
main
```
`init()`は複数書けるが、混乱の元なので一つにまとめることが推奨される。