## React.Fragment
ReactでJSXを記述する際には必ず1つのタグで囲う必要がある。
```jsx
const App = () => {
    return (
        <div>
            <p>hoge</p>
            <p>fuga</p>
            <p>piyo</p>
        </div>
    )
}
```
不要なdivタグを増やしたくない場合、
`<React.Fragment>`か`<>`を使う。
```jsx
const App = () => {
    return (
        <React.Fragment>
            <p>hoge</p>
            <p>fuga</p>
            <p>piyo</p>
        </React.Fragment>
    )
}
```
`<>`は`<React.Fragment>`の短縮記法なので意味は同じ。
```jsx
const App = () => {
    return (
        <>
            <p>hoge</p>
            <p>fuga</p>
            <p>piyo</p>
        </>
    )
}
```

## React.FCによる型注釈
以下のような書き方のほかに、
```jsx
const User = ({ name, age = 20 }: UserType) => {
    // 略
}
```
`React.FC`を使うと以下のように型注釈できる。
```jsx
const User: React.FC<UserType> = ({ name, age = 20 }) => {
    // 略
}
```
ただし、React18以前だと暗黙的に`children`を`props`に含む。
React18からは`children`は含まれない。

## 型
### イベントの型
```jsx
return (
  <div>
    <input onChange={handleChange}></input>
  </div>
);
```
2通りの書き方があり、どっちでもよい。
ハンドラーに型をつける場合は`Hander`と名前がつくだけの違い。
```jsx
const handleChange: React.ChangeEventHandler<HTMLInputElement> = (e) => {
  console.log(e.target.value);
};
```
引数に型をつける場合。
```jsx
const handleChange = (e: React.ChangeEvent<HTMLInputElement>) => {
  console.log(e.target.value);
};
```

### childrenにつける型
```jsx
const ChildComponent: React.FC<{ children: React.ReactNode }> = (props) => {
  return <div>{props.children}</div>;
};
```

## useRef
`ref`属性に渡してやることで、仮想ではない実際のDOMを扱える。
以下はフォームにフォーカスするボタンの例。
```jsx
const inputRef = useRef<HTMLInputElement>(null)

...

const handleFocusButton = () => {
    if (inputRef.current) {
        inputRef.current.focus()
    }
}

...

<input
    type="text"
    ref={inputRef}
/>
<button onClick={handleFocusButton}>focus</button>
```

## useEffect
ネットワーク、ブラウザAPI、サードパーティライブラリとの接続に使う。

レンダーの後に実行される。
注意点として、クリーンアップ関数もレンダーの後に呼ばれる。

## リストのキーにインデックスを使ってはならない
例えば以下のような例はダメ。
```jsx
<ul>
    {itemList.map((item, index) => (
        <Item key={index} {...item} />
    ))}
</ul>
```
表示上は問題なく見えるが、
要素を追加した時の挙動が直感的でなくバグの元になる。

たとえば、`[b,c]`のようなリストの先頭に`a`を追加したとする。
リストは`[a,b,c]`のようになる。
この時、今まで`b`がキーとして`0`を与えられていたのに、新しく`1`がキーになる。`c`も同様にずれる。
すると、Reactは「`a`が追加された」だけでなく、「`b`も`c`も変更された」と判断してしまう。

これによって発生しうる問題は以下。
- パフォーマンス
  - `a`だけレンダーすればいいのに、`b`も`c`もレンダーし直してしまう。
- バグ
  - `a`に対して`useEffect`が発火するだけでなく、`b`と`c`に対しても発火してしまう。

正しくは、リストにユニークな文字列などを入れておいて、それをキーにすべき。
