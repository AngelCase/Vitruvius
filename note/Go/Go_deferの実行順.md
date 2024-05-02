後に記述されたものが先に実行される。
```go
defer fmt.Println("1");
defer fmt.Println("2");
defer fmt.Println("3");
```
出力：
```
3
2
1
```