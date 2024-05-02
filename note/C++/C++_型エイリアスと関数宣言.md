[型エイリアスと関数宣言](https://qiita.com/tetsu0121/items/10b6d3fb2579da1c67fc)

## 概要
関数宣言を簡略化するときに型エイリアスで見やすくできる。
簡略化とは、以下のような変数名の省略を指す：
```c++
uint32_t attack( const uint8_t& strength, const uint8_t& rate );
 ↓
uint32_t attack( const uint8_t&, const uint8_t& );
```
これだと変数名が消えてどういう値を引数として渡せばいいのかわからない。
そこで型エイリアスを使う：
```c++
using Damage = uint32_t;
using Strength = uint8_t;
using Rate = uint8_t;
Damage attack( const Strength&, const Rate& );
```
これで見やすくなった。

## 感想
自分は引数名を省略する習慣がないのでそこは何も感じなかった。
ただ、戻り値は確かに見やすいので採用したい。
