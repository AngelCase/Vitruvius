## 概要
C++やJavaにも`case`はあるが、Haskellはそれらのものよりも一歩先を行っている。

`case`式は`if`や`else`、`let`束縛のように式である。
`case`式は以下のような構文である。
```haskell
case expression of pattern -> result
                   pattern -> result
                   pattern -> result
                   ...
```
なお、`case`は関数のパターンマッチの糖衣構文である。
そのため、以下の2つのコードは同じふるまいをして、入れ替え可能である。
```haskell
head' :: [a] -> a
head' [] = error "No head for empty lists!"
head' (x:_) = x

head' :: [a] -> a
head' xs = case xs of [] -> error "No head for empty lists!"
                      (x:_) -> x
```
上から順にマッチするか確認していき、
マッチするパターンが見つからなければランタイムエラーが発生する。

関数のパターンマッチが関数定義の際にしか使えないのに対して、
`case`式はどこでも使える。
```haskell
describeList :: [a] -> String
describeList xs = "The list is " ++ case xs of [] -> "empty."
                                               [x] -> "a singleton list." 
                                               xs -> "a longer list."
```
関数定義におけるパターンマッチは`case`式の糖衣構文なので、
同じことを以下のように書ける。
```haskell
describeList :: [a] -> String
describeList xs = "The list is " ++ what xs
    where what [] = "empty."
          what [x] = "a singleton list."
          what xs = "a longer list."
```
