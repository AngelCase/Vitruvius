## any
あらゆる型の値を保持できる記憶域型。
```c++
std::any x = 3; // int型の値3で初期化
x = std::string("Hello"); // std::string型の値"Hello"を再代入

// 値を取り出す
std::string s = std::any_cast<std::string>(x);
assert(s == "Hello");
```
`std::shared_ptr<void>`でも同様のことはできるが、その場合はポインタの意味論で値を保持することになる。`any`の場合は値の意味論で値を保持することになる。

## variant
格納されうる候補の型リストに含まれる型のオブジェクトを切り替えながら保持する記憶域型。
```c++
// 継承関係にないクラス群
struct A { void f() {} };
struct B { void f() {} };
struct C { void f() {} };

// A, B, Cのいずれかの型を代入できる型
std::variant<A, B, C> v = A{}; // A型のオブジェクトを代入
v = B{}; // B型のオブジェクトに切り替え

// B型オブジェクトを保持しているか
if (std::holds_alternative<B>(v)) {
  // 保持しているB型オブジェクトを取得
  B& b = std::get<B>(v);
}

// どの型が代入されていたとしても、共通のインタフェースを呼び出す（Visitorパターン）
std::visit([](auto& x) {
  x.f();
}, v);
```

## 比較
`variant`は代入されうる型の候補が静的に既知。
`any`はその候補を実行時まで遅らせられる。

## 参考
https://cpprefjp.github.io/reference/any/any.html
https://cpprefjp.github.io/reference/variant/variant.html
https://cpprefjp.github.io/reference/variant/visit.html
