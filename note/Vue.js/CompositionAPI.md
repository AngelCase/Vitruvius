## 特徴
1. 大規模化への対応
2. [[合成関数]]（コードの再利用性を高める）
3. TSのサポート
また、Vue2のoptionsAPIはそのまま使える。

## OptionsAPIとの比較
OptionsAPIだとdata、methods、computed、mountedなどの役割ごとに場所が分かれていた。
CompositionAPIなら処理ごとにまとめて書けるためコードが増えても見やすい（[[合成関数]]）。
![[OptionsAPIとCompositionAPI.PNG]]

## 主要な機能
[[setup()]]