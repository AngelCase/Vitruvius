例外処理に使う。
`panic()`が呼ばれると`recover()`でその内容が取得できる。
```go
defer func() {
	if x := recover(); x != nil {
		fmt.Println(x)
	}
}()
panic("runtime error")
```
出力：
```
runtime error
```