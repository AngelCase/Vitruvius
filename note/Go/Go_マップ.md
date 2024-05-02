マップは参照型なので普通に代入すると参照渡しになる：
```go
m := map[string]int{"a": 100, "b": 200}
fmt.Println(m)
// 出力：map[a:100 b:200]

m2 := m
m2["hoge"] = 999
fmt.Println(m)
// 出力：map[a:100 b:200 hoge:999]
fmt.Println(m2)
// 出力：map[a:100 b:200 hoge:999]
```
存在しないキーを渡して値の取得に失敗した場合、その型の初期値が帰ってくる。
2つ目の戻り値で成否が得られるので、それを利用してエラーハンドリングができる。
```go
m := map[string]int{"a": 100, "b": 200}

value := m["a"]
fmt.Println(value)
// 出力：100

value, ok := m["c"]
if !ok {
	fmt.Println(value, "error")
// 出力：0 error
}

delete(m, "a")
fmt.Println(m)
// 出力：map[b:200]
```