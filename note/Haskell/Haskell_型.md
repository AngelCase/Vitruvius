## 型の基本
GHCIでは`:t`で式の型を出力できる。
```haskell
ghci> :t 'a'
'a' :: Char

ghci> :t True
True :: Bool

ghci> :t "HELLO!"
"HELLO!" :: [Char]

ghci> :t (True, 'a')
(True, 'a') :: (Bool, Char)

ghci> :t 4 == 5
4 == 5 :: Bool
```
`"HELLO!"`は文字のリストなので、`[Char]`となっている点に注意。

型の名前は大文字から始まる。

## 関数の型
関数も同様に型を持つ。
一般的に、きわめて短い関数でなければ、明示的に型を宣言するのがよい習慣とされる。
```haskell
removeNonUppercase :: [Char] -> [Char]
removeNonUppercase st = [ c | c <- st, c `elem` ['A'..'Z']]
```
複数パラメータを持つ場合は以下のようになる。
```haskell
addThree :: Int -> Int -> Int -> Int
addThree x y z = x + y + z
```

## 型の概観
### Int
整数。
32-bitマシンでは`2147483647`から`-2147483648`の範囲。
## Integer
`Int`同様に整数だが、範囲の制限がない。
なので巨大な数も扱えるが、`Int`の方が効率的である。
```haskell
factorial :: Integer -> Integer
factorial n = product [1..n]

ghci> factorial 50
30414093201713378043612608166064768844377641568960512000000000000
```
### Float
単精度の浮動小数点数。
```haskell
circumference :: Float -> Float
circumference r = 2 * pi * r

ghci> circumference 4.0
25.132742
```
### Double
倍精度の浮動小数点数。
```haskell
circumference' :: Double -> Double
circumference' r = 2 * pi * r

ghci> circumference' 4.0
25.132741228718345
```
### Bool
ブール型。
`True`と`False`のどちらかのみとる。
### Char
文字。シングルクォートで示される。
文字のリストは文字列である。
### Tuple
コンポーネントの長さと型によって型が変わるため理論的には無限の型がある。

## 型変数
関数`head`の型を見てみる。
```haskell
ghci> :t head
head :: [a] -> a
```
`a`というのがあるが、
型の名前は大文字から始まるのでこれは型ではなく、
「型変数」と呼ばれる。
つまり、どんな型にでもなりうる。

型変数をもつ関数は「多相関数」と呼ばれる。
