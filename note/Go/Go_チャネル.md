ゴルーチン間でデータの送受信を行うためのデータ型。
```go
var ch chan int

// 送信専用
var send_ch chan<- int

// 受信専用
var receive_ch <-chan int
```
データ構造としてはキューとなっており、最初に入れたものが最初に取り出される。
```go
ch := make(chan int, 3)

ch <- 1
ch <- 2
ch <- 3

println(<-ch)
println(<-ch)
println(<-ch)
```
キャパシティを超える量を入れてしまうとデッドロックとなってしまう。
```go
// キャパシティは3
ch := make(chan int, 3)

ch <- 1
ch <- 2
ch <- 3
ch <- 4
```
ゴルーチン内のチャネルとやり取りを行うのに使われる：
```go
func receiver(ch chan int) {
	// チャネルの受信した値を出力し続ける
	for {
		fmt.Println(<-ch)
	}
}

func main() {
	ch1 := make(chan int, 3)
	ch2 := make(chan int, 3)

	go receiver(ch1)
	go receiver(ch2)

	for i := 0; i < 100; i++ {
		ch1 <- i
		ch2 <- i
		time.Sleep(100 * time.Millisecond)
	}
}
```

## close
チャネルはcloseという状態を持つ。
close状態でかつ、受信している値がなくなったらチャネルは2つ目の戻り値で`false`を返す。
```go
func receiver(ch chan int) {
	for {
		value, ok := <-ch
		if !ok {
			break
		}
		fmt.Println(value)
	}
	fmt.Println("end")
}

func main() {
	ch := make(chan int, 3)

	go receiver(ch)

	for i := 0; i < 2; i++ {
		ch <- i
	}

	// チャネルをクローズ
	close(ch)
	time.Sleep(3 * time.Second)
}
```
出力：
```
0
1
end
```

## for文との組み合わせ
```go
ch := make(chan int, 3)

ch <- 1
ch <- 2
ch <- 3

// クローズしておかないと、
// チャネルの値がなくなっても読み続けてデッドロックになる
close(ch)

for i := range ch {
	fmt.Println(i)
}
```
出力：
```
1
2
3
```

## select
`switch`と似ているが、`case`がランダムに選ばれるという違いがある。
```go
ch1 := make(chan int, 3)
ch2 := make(chan string, 3)

ch1 <- 1
ch2 <- "hoge"

select {
case i := <-ch1:
	fmt.Println(i)
case str := <-ch2:
	fmt.Println(str)
default:
	fmt.Println("どちらでもない")
}
```
出力：
```
1
```
または：
```
hoge
```
