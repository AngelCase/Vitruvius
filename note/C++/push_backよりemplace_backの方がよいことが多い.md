pushはコピーするけど，emplaceはムーブなので特にコンストラクタが重い場合速度に差が出る。
ただし、以下のようなケースでemplace_backはコンパイルエラーにならないなど、正しく使い分けるには熟慮が必要となる。
```c++
#include <vector>
#include <regex>

int main()
{
  std::vector<std::regex> rv;

  rv.emplace_back(nullptr);  // 実行時エラー
  // rv.push_back(nullptr);  // コンパイルエラー
}
```

## 参考
[3-5. vectorへのemplace_back/push_back説明への違和感 · Issue #72 · rinatz/cpp-book · GitHub](https://github.com/rinatz/cpp-book/issues/72)