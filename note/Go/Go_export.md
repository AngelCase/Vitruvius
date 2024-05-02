Goでは名前が大文字で始まるものはexportされ、外部パッケージからアクセスできる。
例えば、`Pizza`や`Pi`はアクセスできるが、`pi`はアクセスできない。
```go
import (
	"fmt"
	"math"
)

func main() {
	fmt.Println(math.pi)
	// エラー！piが未定義
}
```