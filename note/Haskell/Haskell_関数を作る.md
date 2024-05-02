引数を1つとり、それを2倍にする関数`doubleMe`を作る。
記述の順番は関数名、パラメータ、`=`、実行する内容。
```haskell
doubleMe x = x + x
```
これを`baby.hs`のような名前で保存し、そのディレクトリでGHCIを起動する。
`:l baby`とすればこのファイルの内容を読み込める。
```haskell
ghci> :l baby
[1 of 1] Compiling Main             ( baby.hs, interpreted )
Ok, modules loaded: Main.
ghci> doubleMe 9
18
ghci> doubleMe 8.3
16.6
```
2つの引数を取る例：
```haskell
doubleUs x y = x*2 + y*2
```
実行結果：
```haskell
ghci> doubleUs 4 9
26
ghci> doubleUs 2.3 34.2
73.0
ghci> doubleUs 28 88 + doubleMe 123
478
```
もちろん、関数を別の関数から呼ぶこともできる：
```haskell
doubleUs x y = doubleMe x + doubleMe y
```

そのほか、関数についての注意点として、関数名は大文字から始めることができない。

関数がパラメータを取らない場合、それは「定義」、または「名前」という。

関数はinfix functionとして呼べるだけでなく、
infix functionとして定義もできる。
```haskell
myCompare :: (Ord a) => a -> a -> Ordering
a `myCompare` b
    | a > b     = GT
    | a == b    = EQ
    | otherwise = LT

ghci> 3 `myCompare` 2
GT
```
