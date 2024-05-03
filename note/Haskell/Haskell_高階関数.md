## 高階関数
Haskellの関数は関数をパラメータとしてとることができ、
また関数を戻り値として返せる。
これらのどちらかを行う関数を高階関数という。

これはHaskellの体験の一部というより、
Haskellの体験そのものである。

## カリー化
Haskellの関数は1つのパラメータしかとることができない。
これまで登場した複数のパラメータをとる関数は実はカリー化された関数である。

例えば、`max 4 5`はまず「`4`と与えられたパラメータのうち大きい方を返す関数」を返している。
以下の2つの関数呼び出しは等価である。
```haskell
ghci> max 4 5
5
ghci> (max 4) 5
5
```

空白を2つのものの間に配置するのは
シンプルに関数の適用である。
空白はオペレータのようなもので最高の優先順位をもつ。

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


Take a look at this offensively simple function:

multThree :: (Num a) => a -> a -> a -> a
multThree x y z = x * y * z

What really happens when we do multThree 3 5 9 or ((multThree 3) 5) 9? First, 3 is applied to multThree, because they're separated by a space. That creates a function that takes one parameter and returns a function. So then 5 is applied to that, which creates a function that will take a parameter and multiply it by 15. 9 is applied to that function and the result is 135 or something. Remember that this function's type could also be written as multThree :: (Num a) => a -> (a -> (a -> a)). The thing before the -> is the parameter that a function takes and the thing after it is what it returns. So our function takes an a and returns a function of type (Num a) => a -> (a -> a). Similarly, this function takes an a and returns a function of type (Num a) => a -> a. And this function, finally, just takes an a and returns an a. Take a look at this:

ghci> let multTwoWithNine = multThree 9
ghci> multTwoWithNine 2 3
54
ghci> let multWithEighteen = multTwoWithNine 2
ghci> multWithEighteen 10
180

By calling functions with too few parameters, so to speak, we're creating new functions on the fly. What if we wanted to create a function that takes a number and compares it to 100? We could do something like this:

compareWithHundred :: (Num a, Ord a) => a -> Ordering
compareWithHundred x = compare 100 x

If we call it with 99, it returns a GT. Simple stuff. Notice that the x is on the right hand side on both sides of the equation. Now let's think about what compare 100 returns. It returns a function that takes a number and compares it with 100. Wow! Isn't that the function we wanted? We can rewrite this as:

compareWithHundred :: (Num a, Ord a) => a -> Ordering
compareWithHundred = compare 100

The type declaration stays the same, because compare 100 returns a function. Compare has a type of (Ord a) => a -> (a -> Ordering) and calling it with 100 returns a (Num a, Ord a) => a -> Ordering. The additional class constraint sneaks up there because 100 is also part of the Num typeclass.

_Yo!_ Make sure you really understand how curried functions and partial application work because they're really important!

Infix functions can also be partially applied by using sections. To section an infix function, simply surround it with parentheses and only supply a parameter on one side. That creates a function that takes one parameter and then applies it to the side that's missing an operand. An insultingly trivial function:

divideByTen :: (Floating a) => a -> a
divideByTen = (/10)

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
