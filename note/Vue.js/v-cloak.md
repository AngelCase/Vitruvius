Vue.jsの表示タイミングによってはマスタッシュ構文が一瞬だけ表示されてしまうことがある。
v-cloakではコンパイルされていないマスタッシュを隠すのに使える。
CSS:
```
[v-cloak] {
	diaplay: none;
}
```
HTML:
```
<div v-cloak>
	{{ message }}
</div>
```