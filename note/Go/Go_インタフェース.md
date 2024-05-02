インタフェースはポリモーフィズムを実現するための機能。
インタフェースはメソッドのシグニチャの集合で定義する。
以下の例では、`ToString()`を持つ型であれば`Stringify`型の変数に持たせられる。
```go
type Stringify interface {
	ToString() string
}

type Person struct {
	Name string
	Age  int
}

func (p Person) ToString() string {
	return fmt.Sprintf("Name=%v, Age=%v", p.Name, p.Age)
}

type Car struct {
	Model string
}

func (c Car) ToString() string {
	return fmt.Sprintf("Model=%v", c.Model)
}

func main() {
	stringifySlice := []Stringify{
		Person{"person", 10},
		Car{"car"},
	}

	for _, v := range stringifySlice {
		fmt.Println(v.ToString())
	}
}
```
出力：
```
Name=person, Age=10
Model=car
```
