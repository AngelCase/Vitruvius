## 高階関数
Haskellの関数は関数をパラメータとしてとることができ、
また関数を戻り値として返せる。
これらのどちらかを行う関数を高階関数という。

これはHaskellの体験の一部というより、
Haskellの体験そのものである。

## カリー化
Haskellの関数は1つのパラメータしかとることができない。
これまで登場した複数のパラメータをとる関数は実は「カリー化」された関数である。
カリー化とは、「複数のパラメータをとる関数を
1つのパラメータだけをとる一連の関数の系列に変換すること」である。

カリー化された関数の例として`max`を見る。
例えば、`max 4 5`はまず「`4`と与えられたパラメータのうち大きい方を返す関数」を返している。
以下の2つの関数呼び出しは等価である。
```haskell
ghci> max 4 5
5
ghci> (max 4) 5
5
```

`max`の型は以下である：
```haskell
max :: (Ord a) => a -> a -> a
```
以下のようにも書ける：
```haskell
max :: (Ord a) => a -> (a -> a)
```
つまり、`max`は`a`をとり、`a`をとる関数を返すということである。
これこそが関数のパラメータが矢印で区切られていた理由である。

## 部分適用
関数を返す、ということをわかりやすくするための例を示す。
以下の関数があった時、
```haskell
multThree :: (Num a) => a -> a -> a -> a
multThree x y z = x * y * z
```
以下のように使える。
```haskell
ghci> let multTwoWithNine = multThree 9
ghci> multTwoWithNine 2 3
54
ghci> let multWithEighteen = multTwoWithNine 2
ghci> multWithEighteen 10
180
```
このような、関数のとるパラメータのうち一部のパラメータを渡すことを「部分適用」という。

## セクション
infix functionで部分適用する場合、「セクション」を使う。
セクションを使うにはパラメータを1つだけ適用した関数を括弧で囲う：
```haskell
divideByTen :: (Floating a) => a -> a
divideByTen = (/10)
```
以下の3つは等価である：
```haskell
divideByTen 200
200 / 10
(/10) 200
```

別の例として、与えられた文字が大文字化をチェックする関数は以下のように書ける：
```haskell
isUpperAlphanum :: Char -> Bool
isUpperAlphanum = (`elem` ['A'..'Z'])
```

セクションに関してただ一つ注意すべきは`-`の扱いである。
セクションの定義からいくと、`(-4)`は`4`を引く関数であるように思われるが、
`(-4)`はマイナス4を意味する。
もし、`4`を引く関数を作りたいなら`(subtract 4)`のように書く。
