## 一様初期化
コンストラクタの呼び出しを波括弧`{}`で記述する構文。

## 注意すべき仕様
`std::initializer_list`型を取るコンストラクタと
初期化子リストの要素型と同じ型のパラメータリストを受け取るコンストラクタがあった時、
`std::initializer_list`型を受け取るコンストラクタが優先して呼び出される。
```cpp
struct X {
  X(std::initializer_list<double>) {
    std::cout << 1 << std::endl;
  }
  X(double d) {
    std::cout << 2 << std::endl;
  }
};

X x1 = {3.0}; // 「1」が出力される
```
非`std::initializer_list`のコンストラクタを呼び出す場合、
丸括弧でのコンストラクタ呼び出しが必要。

## 危険なケースの例
`std::vector`には要素数と初期化時の値を取るコンストラクタがあるが、
`std::initializer_list`を取るコンストラクタもあるため注意。
```cpp
vector<int> arr1(1, 2);    // [2]
vector<int> arr2{1, 2};    // [1,2]
vector<int> arr3 = {1, 2}; // [1,2]
```

## 参考
[一様初期化 - cpprefjp C++日本語リファレンス](https://cpprefjp.github.io/lang/cpp11/uniform_initialization.html)
[C++11 Universal Initialization は、いつでも使うべきなのか #C++ - Qiita](https://qiita.com/h2suzuki/items/d033679afde821d04af8)
