## 概要
基準要素より小さい要素が前に来るように並び替える。
それによって以下のような状態となる：
- 基準位置`nth`をソートしたときの位置に持ってくる
- 基準位置より前にはそれより小さい要素だけがある
- 基準位置より前にはそれより大きい要素だけがある
ソートはしない点に注意。

計算量は平均で線形時間（$O(N)$）。

## 使いどころ
「大きい順に上から3個要素を取り出す」みたいなケース。
または、コンテナの広い部分をソートするケース（参考：[[partial_sort対nth_elementとsort]]）。

## 例
以下のプログラムでは0から数えて3番目の要素だけ、ソート後の要素になっている。
```cpp
#include <iostream>
#include <vector>
#include <algorithm>

int main()
{
  std::vector<int> v = {5, 10, 4, 7, 1, 9, 8, 6, 2};

  // 4番目に小さい値より小さい値を前に集める
  std::nth_element(v.begin(), v.begin() + 3, v.end());

  std::for_each(v.begin(), v.end(), [](int x) {
    std::cout << x << std::endl;
  });
}
```
出力：
```
2
1
4
5
10
9
8
6
7
```
今回3番目の要素は`5`。
`[0,3)`の要素は`5`よりも小さく、
`(3,last)`の要素は`5`よりも大きくなっている。
ただし、全体のソートはされていない。

## 参考
[nth_element - cpprefjp C++日本語リファレンス](https://cpprefjp.github.io/reference/algorithm/nth_element.html)
