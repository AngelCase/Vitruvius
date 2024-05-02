関数ポインタは`using`の方が見やすい。
```c++
typedef void (*f)(int, char);
using f = void (*)(int, char);
```

また、テンプレート化も可能。
```c++
template<class Value>
using dict = std::map<std::string, Value>;
```

## 参考
https://qiita.com/Linda_pp/items/44a67c64c14cba00eef1