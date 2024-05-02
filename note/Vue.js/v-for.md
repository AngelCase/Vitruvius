for文のような操作ができる：
```
<ul>
	<li v-for="member in members">
		{{ member }}
	</li>
</ul>
...
members: ['本田', '香川', '長友']
```
要素番号（0から始まる）も取れる：
```
<ul>
	<li v-for="(member, index) in members">
		{{ index }} : {{ member }}
	</li>
</ul>
```
オブジェクトのキーとバリューも取れる：
```
<ul>
	<li v-for="(value, key, index) in book">
		{{ index }} : {{ key }} : {{ value }}
	</li>
</ul>
...
book: {
	title: '本のタイトル',
	url: 'https://google.com',
},
```
オブジェクトを渡す場合、どの順番で表示するのかを決めてやる必要がある。
そのためにkeyという属性を使う：
```
<div v-for="item in items" :key="item.id">
	{{ item.text }}
</div>
```