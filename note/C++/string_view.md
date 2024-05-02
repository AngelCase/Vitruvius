## 概要
文字列に対する読み取り専用のビュー。
ポインタっぽい概念のため、参照先の文字列の寿命が切れたらダングリングポインタっぽくなって危険。
そのため、関数の引数として使うのはいいが戻り値として使うなら十分注意すること。

## 関連
`std::span`も同様に、シーケンスの所有権を保持せず部分シーケンスを参照するクラスとなっている。
文字列操作なら`std::string_view`で、それ以外のメモリ連続性をもつあらゆるコンテナに`std::span`は対応している。

## 参考
[文字列ビューstd::string_view 利用ガイド - yohhoyの日記 (hatenadiary.jp)](https://yohhoy.hatenadiary.jp/entry/20180810/p1)
[basic_string_view - cpprefjp C++日本語リファレンス](https://cpprefjp.github.io/reference/string_view/basic_string_view.html)
[span - cpprefjp C++日本語リファレンス](https://cpprefjp.github.io/reference/span/span.html)