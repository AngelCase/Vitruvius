## テストのやり方
以下のような`main.go`ファイル内の関数をテストしたいとする。
```go
func IsOne(i int) bool {
	return i == 1
}
```
その場合、`main_test.go`を作成し、以下のように記述する。
```go
import "testing"

func TestIsOne(t *testing.T) {
	v := IsOne(1)

	if !v {
		t.Errorf("%v != %v", v, 1)
	}
}
```
次のコマンドでテスト実行できる。
```sh
go test
```
出力：
```
PASS
ok      go-todo-web     0.001s
```

## その他
次のコマンドでディレクトリ内のすべてのパッケージのテストを実行できる。
```sh
go test ./...
```
次のコマンドでカバー率を出力できる。
```sh
go test -cover
```
