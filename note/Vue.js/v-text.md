要素のtextContentを更新する。
タグが含まれていても文字として扱う。
```
<div v-html="test"></div>
...
test: 'あしたは <br> はれ'
```