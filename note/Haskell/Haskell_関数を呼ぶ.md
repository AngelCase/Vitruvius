## 関数を呼ぶ
Haskellの関数はスペース区切りで引数を取るので注意。
関数の例（`succ`）。
与えられた引数に+1する。
```haskell
ghci> succ 8
9
```
関数の例（`max`と`min`）。
```haskell
ghci> min 9 10
9
ghci> min 3.4 3.2
3.2
ghci> max 100 101
101
```

## 関数の優先順位
関数の優先順位は最も高いので、以下の2つの例は等価。
```haskell
ghci> succ 9 + max 5 4 + 1
16
ghci> (succ 9) + (max 5 4) + 1
16
```
`succ 9 * 10`のような例だと`succ 9`が先に評価され、そのあと`* 10`がされるので注意。

## infix function
関数が2つのパラメータを取る場合、バッククォートで囲うとinfix functionとして呼べる。
割算だと、どっちが割る数でどっちが割られる数か分かりにくくなりがちだが、
infix functionにするとより分かりやすくなる。
```haskell
ghci> div 10 2
5
ghci> 10 `div` 2
5
```

オペレーター（`==`、`+`、`*`、`-`、`/`）は関数である。
関数が特殊文字だけで構成される場合、infix functionであるとデフォルトで判断される。
prefix functionとして呼ぶ場合や、他の関数に渡す場合
括弧で囲う必要がある。
```haskell
ghci> (==) 1 2
False
ghci> (+) 1 1 
2
```
