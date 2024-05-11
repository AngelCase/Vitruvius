外部システムとコンポーネントを同期させるためのReactフック。
```jsx
useEffect(setup, dependencies?)
```

`dependencies`に渡した配列の値が変わった時に`setup`が実行される。
一方、`dependencies`を省略すると再レンダーするたびに実行される。

## 応用
[[React_一度だけ実行する処理]]

## 参考
[useEffect – React](https://ja.react.dev/reference/react/useEffect#usage)
[React hooksを基礎から理解する (useEffect編) #JavaScript - Qiita](https://qiita.com/seira/items/e62890f11e91f6b9653f)
