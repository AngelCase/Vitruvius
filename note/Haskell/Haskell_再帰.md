## 再帰
再帰は数学の定義としてよく出てくる。
フィボナッチ数列がその最たる例である。

再帰はHaskellにおいて重要である。
なぜなら、なにか計算を行う際に
命令型言語では「どうやってそれを手に入れるか」を宣言するのに対して、
Haskellでは「それが何か」を宣言するためである。
これはHaskellに`while`ループがない理由である。

## 再帰的に考える
問題を再帰的に考えるとき、以下のことを考える
- 再帰的な解決方法を適用されないケース
- それをエッジケースとして使えるか
- 関数のパラメータをバラバラにするかどうか（例：リストは先頭と末尾に分割される）

## 再帰の実装の例
### maximum
`maximum`関数は順序付け可能なもののリストを取り、
その中の最大のものを返す。

`max`関数を使い、以下のように書ける：
```haskell
maximum' :: (Ord a) => [a] -> a
maximum' [] = error "maximum of empty list"
maximum' [x] = x
maximum' (x:xs) = max x (maximum' xs)
```

### replicate
`replicate`は、1つの`Int`と要素をとり、同じ要素が繰り返しになったリストを返す。
例えば、`replicate 3 5`は`[5,5,5]`を返す。
これを再帰的に実装する。
```haskell
replicate' :: (Num i, Ord i) => i -> a -> [a]
replicate' n x
    | n <= 0    = []
    | otherwise = x:replicate' (n-1) x
```

### take
リストから特定の個数の要素を取り出す関数`take`を実装する。
例えば、`take 3 [5,4,3,2,1]`は`[5,4,3]`を返す。

空のリストの場合と個数が`0`の場合がエッジケースなので以下のように実装できる。
```haskell
take' :: (Num i, Ord i) => i -> [a] -> [a]
take' n _
    | n <= 0   = []
take' _ []     = []
take' n (x:xs) = x : take' (n-1) xs
```

### reverse
`reverse`はリストを逆順にする。
```haskell
reverse' :: [a] -> [a]
reverse' [] = []
reverse' (x:xs) = reverse' xs ++ [x]
```

### repeat
`repeat 3`は3で始まる無限のリストを返す。
```haskell
repeat' :: a -> [a]
repeat' x = x:repeat' x
```

### zip
`zip`は2つのリストをとり合体させる。
`zip [1,2,3] [2,3]`は`[(1,2),(2,3)]`を返す（長い方は切り詰められる）。
```haskell
zip' :: [a] -> [b] -> [(a,b)]
zip' _ [] = []
zip' [] _ = []
zip' (x:xs) (y:ys) = (x,y):zip' xs ys
```

### elem
`elem`は要素とリストをとり、要素がリストにあるかを返す。
```haskell
elem' :: (Eq a) => a -> [a] -> Bool
elem' a [] = False
elem' a (x:xs)
    | a == x    = True
    | otherwise = a `elem'` xs 
```
## クイックソート
クイックソートの実装は命令型言語だと10行はかかるが
Haskellだとより短くエレガントに書ける。

エッジケースはそのまま空のリスト。
メインのアルゴリズムは以下：
- リストの先頭以下の値の要素を先頭に持つ（それらはソートされている）
- リストの先頭より大きい値をその後ろに持ってくる
```haskell
quicksort :: (Ord a) => [a] -> [a]
quicksort [] = []
quicksort (x:xs) = 
    let smallerSorted = quicksort [a | a <- xs, a <= x]
        biggerSorted = quicksort [a | a <- xs, a > x]
    in  smallerSorted ++ [x] ++ biggerSorted
```
以下のように動作する：
```haskell
ghci> quicksort [10,2,5,3,1,6,7,4,2,3,4,8,9]
[1,2,2,3,3,4,4,5,6,7,8,9,10]
ghci> quicksort "the quick brown fox jumps over the lazy dog"
"        abcdeeefghhijklmnoooopqrrsttuuvwxyz"
```
