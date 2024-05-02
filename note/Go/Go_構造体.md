## 基本
```go
type User struct {
	Name string
	Age  int
}

func main() {
	user1 := User{"Adam", 20}
	fmt.Println(user1)
	// 出力：{Adam 20}

	// 順序が異なるためエラー
	// user2 := User{20, "adam"}
	// fmt.Println(user2)

	user3 := User{Name: "Eve", Age: 24}
	fmt.Println(user3)
	// 出力：{Eve 24}
}
```
構造体は値型なので値渡しされる。
```go
type User struct {
	Name string
	Age  int
}

func UpdateUser(user User) {
	user.name = "Jack"
}

func UpdateUserV2(user *User) {
	user.name = "Jack"
}

func main() {
	user3 := User{Name: "Eve", Age: 24}
	fmt.Println(user3)

	// 値渡し
	UpdateUser(user3)
	fmt.Println(user3)
	// 出力：{Eve 24}

	// 参照渡し
	UpdateUserV2(&user3)
	fmt.Println(user3)
	// 出力：{Jack 24}
}
```

## メソッド
Goにクラスはないが、型にはメソッドを付けられる。
その場合、レシーバ引数を記述する（今回は`u`という名前の`User`型レシーバ）。
また、レシーバに変更を加えたい場合、ポインタレシーバを使う。
```go
type User struct {
	Name string
	Age  int
}

// 変数レシーバ
func (u User) SayName() {
	fmt.Println(u.Name)
}

// ポインタレシーバ
func (u *User) SetName(name string) {
	u.Name = name
}

func main() {
	user := User{"Adam", 20}

	user.SayName()
	// 出力：Adam

	user.SetName("Eve")
	user.SayName()
	// 出力：Eve
}
```

## 埋め込み
構造体を他の構造体に埋め込むことができる。
```go
type T struct {
	User
}

type User struct {
	Name string
	Age  int
}

func main() {
	t := T{User{"Adam", 20}}
	fmt.Println(t)
	// 出力：{{Adam 20}}

	// 直接Userのフィールドにアクセス
	// この記法はUser Userと記述していた場合使えない
	fmt.Println(t.Name)
	// 出力：Adam
}
```

複数構造体を埋め込んで、そこに同名フィールドがあった場合、明示的にどちらの構造体のフィールドかを指定する必要がある。
```go
type S1 struct {
	DuplicatedKey string
	V1            int
}

type S2 struct {
	DuplicatedKey string
	V2            int
}

type Embedding struct {
	*S1
	*S2
}

func main() {
	s1 := &S1{
		DuplicatedKey: "s1key",
		V1:            1,
	}
	s2 := &S2{
		DuplicatedKey: "s2key",
		V2:            2,
	}
	e := &Embedding{
		s1,
		s2,
	}

	fmt.Println("---s1を明示的に指定---")
	fmt.Printf("e.S1.duplicatedKey: %+v\n", e.S1.DuplicatedKey)
	// 出力：e.S1.duplicatedKey: s1key

	fmt.Println("---s2を明示的に指定---")
	fmt.Printf("e.S2.DuplicatedKey: %+v\n", e.S2.DuplicatedKey)
	// 出力：e.S2.DuplicatedKey: s2key

	fmt.Println("---明示的に指定しない---")
	fmt.Printf("e.V1: %+v\n", e.V1)
	fmt.Printf("e.V2: %+v\n", e.V2)
	// 出力：e.V1: 1
	// 出力：e.V2: 2

	// エラー、どちらのフィールドかあいまいなため
	// fmt.Println("---名前が衝突しているフィールド---")
	// fmt.Printf("e.DuplicatedKey: %+v\n", e.DuplicatedKey)
}
```
[参考](https://techblog.kayac.com/go-embedding-structs-including-common-keys)

## コンストラクタパターン
Goにはコンストラクタの機能は存在しないが、それを模したコンストラクタパターンはよく用いられる。
```go
type User struct {
	name string
	age  int
}

func NewUser(name string, age int) *User {
	return &User{name, age}
}

func main() {
	user := NewUser("Adam", 20)
	fmt.Println(*user)
	// 出力：{Adam 20}
}
```

## 構造体をスライスに入れる
こちらもよく使われるパターン。
```go
type User struct {
	name string
	age  int
}

type Users []*User

func main() {
	user1 := User{"user1", 10}
	user2 := User{"user2", 20}
	user3 := User{"user3", 30}
	user4 := User{"user4", 40}

	users := make(Users, 0)

	users = append(users, &user1)
	users = append(users, &user2)
	users = append(users, &user3)
	users = append(users, &user4)

	for _, user := range users {
		fmt.Println(*user)
	}
	// 出力：
	// {user1 10}
	// {user2 20}
	// {user3 30}
	// {user4 40}
}
```