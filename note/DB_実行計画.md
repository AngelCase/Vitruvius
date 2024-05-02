## 実行計画の使い方
クエリの頭に`explain`とつけると実行計画が見れる。
```sql
mysql> explain select * from user where name = 'xzrwrthlpi';
```
実行計画は本当に検索はしない。

実行計画を見る際のポイントは、typeとrows。
typeはデータをどう探すのかが書いてある。
例えば、
- `ref`：インデックスを使う
- `range`：インデックスで範囲検索する
- `ALL`：インデックスを使っていない

rowsはどれくらいの行の中から探しているかが書いてある。
少ないほど探す候補が少ないので、小さい方がよい。
なお、予測なので実際の件数とは一致しない点に注意。

## インデックスの効果を実行計画から見る
以下のようなインデックスを作る。
```sql
mysql> alter table user add index i_name(name);
mysql> alter table user add index i_gender(gender);
mysql> alter table user add index i_age(age);
```
`name`で完全一致検索をしてみる。
```sql
mysql> explain select * from user where name = 'xzrwrthlpi';
```
インデックスを作る前だと：
```sql
+------+--------+
| type | rows   |
+------+--------+
| ALL  | 997458 |
+------+--------+
```
インデックスを作った後：
```sql
+------+------+
| type | rows |
+------+------+
| ref  |    1 |
+------+------+
```
このように、インデックスが効いて1行だけ調べればよくなっている。

## 参考
[図解 DB インデックス (zenn.dev)](https://zenn.dev/suzuki_hoge/books/2022-12-database-index-9520da88d02c4f)
