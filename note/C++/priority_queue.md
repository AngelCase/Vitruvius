優先順位付きキュー。
つまり、取り出す順序にルールを決められるキュー。

例えば、「昇順に取り出す」みたいなルールを決められるので、
ソートしたいときに使える。

## 基本的な使い方
デフォルトだと降順。
```cpp
std::priority_queue<int> que;

// データを追加する
que.push(3);
que.push(1);
que.push(4);

// 処理順に出力する
while (!que.empty()) {
std::cout << que.top() << std::endl;
que.pop();
}
```
出力：
```
4
3
1
```

## 比較関数のカスタマイズ
処理の順序はカスタマイズ可能。
ただし、`priority_queue`は末尾から要素を取り出していく点に注意。
つまり、1234の順にソートしたら4321の順に取り出される。

### std::greater
昇順に取り出せる。
```cpp
{
std::priority_queue<
  int,                // 要素の型はint
  std::vector<int>,   // 内部コンテナはstd::vector (デフォルトのまま)
  std::greater<int>   // 昇順 (デフォルトはstd::less<T>)
> que;

que.push(3);
que.push(1);
que.push(4);

while (!que.empty()) {
  std::cout << que.top() << std::endl;
  que.pop();
}
}
```
出力：
```
1
3
4
```

### ラムダ式
比較関数にラムダ式を渡すこともできる。
```cpp
auto compare = [](int a, int b) {
  return a < b;
};

std::priority_queue<
  int,
  std::vector<int>,
  decltype(compare) // 比較関数オブジェクトを指定
> que {compare};

que.push(3);
que.push(1);
que.push(4);

while (!que.empty()) {
  std::cout << que.top() << std::endl;
  que.pop();
}
```
出力：
```
4
3
1
```
`decltype`はオペランドで指定した式の型を取得する機能。
テンプレートにラムダ式の型を渡すために使っている。

また、コンストラクタは引数に比較関数をとっている。

## 参考
[priority_queue - cpprefjp C++日本語リファレンス](https://cpprefjp.github.io/reference/queue/priority_queue.html)
[decltype - cpprefjp C++日本語リファレンス](https://cpprefjp.github.io/lang/cpp11/decltype.html)
