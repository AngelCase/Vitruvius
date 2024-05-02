## 概要
構造体は以下のように宣言できる。
```c
struct SStudent
{
    char szName[16];
    int  nJapanese;
    int  nMath;
    int  nEnglish;
};
```
Cから存在する機能として、無名構造体がある。
```c
struct
{
    char szName[16];
    int  nJapanese;
    int  nMath;
    int  nEnglish;
} student;
```

## 使い道
### `tuple`の代用
`tuple`との差は要素に名前を付けられる点。
```c++
auto getDate() {
  // 無名構造体
  struct { int year, month, day; } date = {2016, 12, 24};
  return date;
}

auto date = getDate();
```
注意点として、引数として渡そうとするとテンプレートを使う必要がある。
```c++
template<class T> void printDate(const T& date) {
  printf("%d-%d-%d\n", date.year, date.month, date.day);
}

printDate(getDate()); // "2016-12-24"
```
このように、無名構造体は型の情報があいまいなので、引数型としてやり取りしようとすると不便。

### 共用体の中で使う
```c++
union UDWord
{
    unsigned long l;       // x.l   １ダブルワード（１ダブルワード＝４バイト）
    struct
    {
        unsigned short l;  // x.s.l 下位ワード
        unsigned short h;  // x.s.h 上位ワード
    } s;                   //       ２ワード（１ワード＝２バイト）
};
```

## 参考
[【C++】無名構造体で名前付きタプルを実現する | MaryCore](https://marycore.jp/prog/cpp/anonymous-struct-tuple/)
http://www7b.biglobe.ne.jp/~robe/cpphtml/html03/cpp03012.html