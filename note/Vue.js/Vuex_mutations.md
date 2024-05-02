---
aliases: [mutations]
---
[[Vuex_state|state]]を変えるためのメソッド。
```
$store.commit('increment', {
	value: 10
})
```
store内：
```
mutations: {
	addCount( state, payload ) {
		state.count += payload.value
	}
}
```