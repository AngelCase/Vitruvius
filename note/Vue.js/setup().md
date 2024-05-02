data()と並列の位置にsetup()を書く：
```
<template>
	{{ name }}
</template>

setup() {
	let name = '大谷'
	return { name }
}
```
変数や関数をreturnすることでtemplate内で使えるようににする。
ただし、このままではリアクティブでなく、値を変更することなどはできないので、[[リアクティビティーAPI]]を使う。
setup()はcreated()よりも先に実行される。

computedやwatchはsetup()内で書ける。

## 注意
setup関数内ではthisは使わないこと。実行タイミングの関係でthisはundefinedになっている。

## 第一引数：props
引数として親から渡ってきたpropsを使える。
```
setup(props) {
	console.log(props.xxx)
}
```

## 第二引数：context
thisは使えないため、代わりにcontextオブジェクトで指定する。
```
setup(props, context) {
	console.log(context.emit)
	console.log(context.attr)
	console.log(context.slots)
}
```

contextはJSオブジェクトであるため分割代入可能。
```
setup(props, { attr, slots, emit }){
	console.log(emit())
}
```

propsが不要な場合は_で第一引数を無視できる。
```
setup(_,{emit}) {
	...
}
```


## setup内で使えるメソッド
[[Vue3_computed|computed]]
[[Vue3_watch|watch]]
[[Vue3_watchEffect|watchEffect]]

## setup内のライフサイクルフック
Vue2のライフサイクルフックの頭にonが付く形となっている。
（例：mounted → onMounted）
```
onMounted(() => {
	console.log('onMounted')
})
```

## [[Vue_Router]]を使う
useRouterやuseRouteという合成関数をimportして使う。
```
import { useRouter, useRoute } from 'vue-router'
setup() {
	const router = useRouter()
	const route = useRoute()

	const goHome = () => {
		router.push('/')
	}
	ocnst checkRoute = () => {
		console.log(route.path)
	}

	return {
		goHome, checkRoutePath
	}
}
```

## [[ナビゲーションガード]]を読み込む
Vue2のナビゲーションガードの頭にonを付ける形式。
```
import { onBeforeRouteLeave, onBeforeRouteUpdate } from 'vue-router'
setup() {
	onBeforeRouteLeave((to, from) => {
		console.log('to: ${to}')
		console.log('from: ${from}')
	})

	onBeforeRouteUpdate((to, from) => {
	})

	return {}
}
```

## [[Vuex]]を使う
useStore()を使うだけで、それ以外の書き方は今までと同じ。
```
import { useStore } from 'vuex'
setup() {
	const store = useStore()

	return {
		// stateにアクセス
		count: computed(() => store.state.count),
		// getterにアクセス
		double: computed(() => store.getters.double)
		// mutationにアクセス
		increment: () => store.commit('increment')
		// actionにアクセス
		asyncIncrement: () => store.dispatch('asyncIncrement')
	}
}
```