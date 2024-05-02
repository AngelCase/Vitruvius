---
aliases: [acitons]
---
非同期処理のための機能だが、同期・非同期でacitons/mutationsを使い分けると混乱の元となるので、同期処理でもactions→[[Vuex_mutations|mutations]]→[[Vuex_state|state]]の流れで書くようにする。
```
$stores.dispatch('incrementAction', {
	value: 10 
})

```
store内：
```
actions: {
	addCountAction(( { commit }, payload)) {
	commit('addCount', payload)
	}
}
```