HTMLタグの属性の中でVueの値を使える。
```
<a v-bind:href="google">googleへのリンク</a>
```
省略記法：
```
<a :href="google">googleへのリンク</a>
```
タグの属性をまとめて指定することもできる：
```
<input v-bind="formInput">
...
formInput: {
	name: 'your_name',
	placeholder: 'お名前を入力してください'
}
</script>
```
属性にはfont-sizeのようにケバブケースのものがあるが、オブジェクト名にハイフンは使えない。
そのため、fontSizeのようにキャメルケースを使う。
あるいは、'font-size'のようにシングルクォーテーションで囲ってやる必要がある。

クラスをつけたり消したりするには以下のようにする：
```
<div :class="{active: active === item.id}"></div>
```
左側にクラス名、右側にその条件、という形式になっている。