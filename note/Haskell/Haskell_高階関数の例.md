## applyTwice
関数をとりそれを2回適用する関数をつくる。
```haskell
applyTwice :: (a -> a) -> a -> a
applyTwice f x = f (f x)
```
これまで括弧は不要だったが、`->`は右結合なので関数を渡す都合上必要となる。
参考：右結合は以下を満たす
```
A・B・C=A・(B・C)
```
[演算子の右結合性、左結合性とは - Panda Noir](https://www.pandanoir.info/entry/2016/06/26/115235)

実行結果：
```haskell
ghci> applyTwice (+3) 10
16
ghci> applyTwice (++ " HAHA") "HEY"
"HEY HAHA HAHA"
ghci> applyTwice ("HAHA " ++) "HEY"
"HAHA HAHA HEY"
ghci> applyTwice (multThree 2 2) 9
144
ghci> applyTwice (3:) [1]
[3,3,1]
```

## zipWith
関数と2つのリストをとり、
リストに関数を適用して結合させる関数を作る。
```haskell
zipWith' :: (a -> b -> c) -> [a] -> [b] -> [c]
zipWith' _ [] _ = []
zipWith' _ _ [] = []
zipWith' f (x:xs) (y:ys) = f x y : zipWith' f xs ys
```

```haskell
ghci> zipWith' (+) [4,2,5,6] [2,6,2,3]
[6,8,7,9]
ghci> zipWith' max [6,3,2,1] [7,3,1,5]
[7,3,2,5]
ghci> zipWith' (++) ["foo ", "bar ", "baz "] ["fighters", "hoppers", "aldrin"]
["foo fighters","bar hoppers","baz aldrin"]
ghci> zipWith' (*) (replicate 5 2) [1..]
[2,4,6,8,10]
ghci> zipWith' (zipWith' (*)) [[1,2,3],[3,5,6],[2,3,4]] [[3,2,2],[3,4,5],[5,4,3]]
[[3,4,6],[9,20,30],[10,12,12]]
```

## flip
関数を受け取り、2つの引数を入れ替えて返す。
```haskell
flip' :: (a -> b -> c) -> b -> a -> c
flip' f y x = f x y
```
実行結果
```haskell
ghci> flip' zip [1,2,3,4,5] "hello"
[('h',1),('e',2),('l',3),('l',4),('o',5)]
ghci> zipWith (flip' div) [2,2..] [10,8,6,4,2]
[5,4,3,2,1]
```
