外部システムとコンポーネントを同期させるためのReactフック。
```jsx
useEffect(setup, dependencies?)
```

`dependencies`に渡した配列の値が変わった時に`setup`が実行される。
一方、`dependencies`を省略すると再レンダーするたびに実行される。

## 注意
開発環境だと処理は2回実行される。
そのため、セットアップ→クリーンアップ→セットアップのように処理を書くことを
公式は推奨している。

例えば、何かに接続するのならクリーンアップで切断する処理を書いておく。
つまり、接続→切断→接続のように実行されるように書く。
```jsx
useEffect(() => {  
    const dialog = dialogRef.current;  
    dialog.showModal();  
    return () => dialog.close();  
}, []);
```

## 応用
[[React_一度だけ実行する処理]]

## 参考
[useEffect – React](https://ja.react.dev/reference/react/useEffect#usage)
[React hooksを基礎から理解する (useEffect編) #JavaScript - Qiita](https://qiita.com/seira/items/e62890f11e91f6b9653f)
[エフェクトを使って同期を行う – React](https://ja.react.dev/learn/synchronizing-with-effects#how-to-handle-the-effect-firing-twice-in-development)
