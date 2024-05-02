```go
Loop:
	for {
		for{
			for{
				fmt.Println("START")
				break Loop
			}
			fmt.Println("ここはスキップ")
		}
		fmt.Println("ここはスキップ")
	}
	fmt.Println("END")
```
実行結果：
```
> go run main.go
START
END
```
見た目的にbreakした後もまたforが始まりそうに見えるが、ラベルはそのあとに続く文を指示するので問題なく終了する。