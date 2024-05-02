グローバルなCSSは非推奨：
```
<style></style>
```
これで書く場合はBEMなどの整理の工夫が必要。

scopedをつけることで有効範囲をコンポーネント内だけにおさめることができる：
```
<style scoped></style>
```