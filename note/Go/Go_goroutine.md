## 概要
goroutineとは並行処理を行うための仕組みで、ランタイムに管理される軽量なスレッド。

## 平行処理でない例
以下の例だと、`sub()`が実行し終わってから`main()`内の`for`が実行される：
```go
func sub() {
	for i := 0; i < 3; i++ {
		fmt.Println("Sub loop")
		time.Sleep(200 * time.Millisecond)
	}
}

func main() {
	sub()

	for i := 0; i < 3; i++ {
		fmt.Println("Main loop")
		time.Sleep(100 * time.Millisecond)
	}
}
```
出力：
```
Sub loop
Sub loop
Sub loop
Main loop
Main loop
Main loop
```

## goroutineを使った並行処理の例
`go`をつけてやるだけで新しいgoroutineが実行される。
```go
func main() {
	go sub()	// goroutine

	for i := 0; i < 3; i++ {
		fmt.Println("Main loop")
		time.Sleep(100 * time.Millisecond)
	}
}
```
出力：
```
Main loop
Sub loop
Main loop
Main loop
Sub loop
```
`sub()`側の`for`の実行回数がひとつ少ないが、これはgoroutineの終了を待たずに`main()`が終了してしまっているせいだと推測される。
![[goroutineとmain終了.png]]
[画像引用元](https://zenn.dev/farstep/articles/f712e05bd6ff9d)
