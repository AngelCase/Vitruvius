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

## infix functionでの部分適用
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

Calling, say, divideByTen 200 is equivalent to doing 200 / 10, as is doing (/10) 200. A function that checks if a character supplied to it is an uppercase letter:

isUpperAlphanum :: Char -> Bool
isUpperAlphanum = (`elem` ['A'..'Z'])

The only special thing about sections is using -. From the definition of sections, (-4) would result in a function that takes a number and subtracts 4 from it. However, for convenience, (-4) means minus four. So if you want to make a function that subtracts 4 from the number it gets as a parameter, partially apply the subtract function like so: (subtract 4).

What happens if we try to just do multThree 3 4 in GHCI instead of binding it to a name with a _let_ or passing it to another function?

ghci> multThree 3 4
<interactive>:1:0:
    No instance for (Show (t -> t))
      arising from a use of `print' at <interactive>:1:0-12
    Possible fix: add an instance declaration for (Show (t -> t))
    In the expression: print it
    In a 'do' expression: print it

GHCI is telling us that the expression produced a function of type a -> a but it doesn't know how to print it to the screen. Functions aren't instances of the Show typeclass, so we can't get a neat string representation of a function. When we do, say, 1 + 1 at the GHCI prompt, it first calculates that to 2 and then calls show on 2 to get a textual representation of that number. And the textual representation of 2 is just the string "2", which then gets printed to our screen.

## Some higher-orderism is in order

Functions can take functions as parameters and also return functions. To illustrate this, we're going to make a function that takes a function and then applies it twice to something!

[Higher Order Functions - Learn You a Haskell for Great Good!](https://learnyouahaskell.com/higher-order-functions)
