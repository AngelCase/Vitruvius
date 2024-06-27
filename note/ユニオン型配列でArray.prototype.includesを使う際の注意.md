ある値がユニオン型の配列に含まれるか確かめたい時、
`includes`をそのまま使おうとするとエラーになる。
```ts
const texts = ["hoge", "fuga"] as const;
texts.includes("piyo");
```
エラー：
```
型 '"piyo"' の引数を型 '"hoge" | "fuga"' のパラメーターに割り当てることはできません。ts(2345)
```

`includes`のシグネチャは次のようになっており、
配列に含まれる要素しか引数に渡すことができない。
```ts
interface ReadonlyArray<T> {
    /**
     * Determines whether an array includes a certain element, returning true or false as appropriate.
     * @param searchElement The element to search for.
     * @param fromIndex The position in this array at which to begin searching for searchElement.
     */
    includes(searchElement: T, fromIndex?: number): boolean;
}
```

次のように、配列側を広げてやることでエラーは解消される。
```ts
const texts = ["hoge", "fuga"] as const;
(texts as readonly string[]).includes("piyo");
```

### 参考
https://stackoverflow.com/questions/56565528/typescript-const-assertions-how-to-use-array-prototype-includes
https://zenn.dev/sushidesu/articles/discussion-of-type-inference-for-array-includes
