## 環境構築
WSLでGoを扱うことにする。
以下のURLにしたがい環境構築を進める。
[Download and install - The Go Programming Language](https://go.dev/doc/install)
[ダウンロードページ](https://go.dev/dl/)から欲しいものをみつけてwgetでダウンロード。
```sh
wget https://go.dev/dl/go1.20.4.linux-amd64.tar.gz
```
sudoを付けて展開。
```sh
 rm -rf /usr/local/go && sudo tar -C /usr/local -xzf go1.20.4.linux-a
md64.tar.gz
```
.zshrcに以下を追加。
```sh
export PATH=$PATH:/usr/local/go/bin
```
インストールされたことを確認。
```
> go version
go version go1.20.4 linux/amd64
```

## 初めて知ったこと
### 言語仕様
[[Go_interface{}]]
[[Go_無名関数]]
[[Go_switch]]
[[Go_ラベル付きfor]]
[[Go_deferの実行順]]
[[Go_panic_recover]]
[[Go_goroutine]]
[[Go_init]]
[[Go_スライス]]
[[Go_可変長引数]]
[[Go_マップ]]
[[Go_チャネル]]
[[Go_ポインタ]]
[[Go_構造体]]
[[Go_インタフェース]]
[[Go_Stringer]]
[[Go_export]]

### 引数の型が同じ場合まとめられる
以下のように引数が同じ型を取る場合、
```go
func Plus(x int, y int) int {
	return x + y
}
```
以下のようにまとめられる。
```go
func Plus(x, y int) int {
	return x + y
}
```

### 関数の戻り値に名前をつけられる
以下の例の場合`result`という名前を付けている。
```go
func Double(price int) (result int) {
	result = price * 2
	return
}
```
Named result parametersという。
[参考](https://go.dev/doc/effective_go#named-results)

## ツール
[[Go_テスト]]
- ソースコードの整形: `go fmt`

### パッケージ
[[Go_os]]