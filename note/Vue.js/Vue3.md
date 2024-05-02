## 特徴
1. 高速
2. ファイルサイズ減
3. [[Provide_Inject]]（長距離のprops）
4. [[CompositionAPI]]（大規模化への対応）
5. TSサポートの改善
6. IE11未対応

## その他機能
[[Teleport]]: 親子関係を飛び越えて表示できる機能

## Vue2との違い
### template直下の書き方
template直下に複数のタグを記述できるので、Vue2のようにdivで囲ってやる必要がない。

### オブジェクトの書き方
以下の例のようにキーとバリューが同じ場合を考える：
```
{
	name: name,
	age: age
}
```
Vue3の場合省略可能：
```
{
	name,
	age
}
```
