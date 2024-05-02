## 概要
オブジェクト指向プログラミング言語から来た人は
型クラスをクラスととらえて混乱しがちだが、
クラスとは違う。
Javaのインタフェースととらえたほうが良い。

関数`==`の型シグネチャを確認する。
```haskell
ghci> :t (==)
(==) :: (Eq a) => a -> a -> Bool
```
`=>`の前にあるものは「クラス制約」と呼ばれる。
この型宣言から読み取れることは以下。
- 関数は同じ型の2つの値をとり、`Bool`を返す
- これらの2つの値は`Eq`クラスのメンバでなければならない

型クラス`Eq`は等価性の確認のためのインタフェースを提供する。
2つの値の等価性を確認することに意味のある型は`Eq`のメンバである必要がある。

## いろんな型クラス
### Eq
`Eq`は型の等価性の確認をサポートするために使われる。

そのメンバが実装する関数は`==`と`/=`である。
そのため、関数の型変数へのクラス制約がある場合、
その関数は定義のどこかで`==`か`/=`を使っている。
### Ord
順序を持つ型につけられる。
```haskell
ghci> :t (>)
(>) :: (Ord a) => a -> a -> Bool
```
`Ord`のメンバになるには、`Eq`のメンバである必要がある。

### Ordering
`GT`、`LT`、`EQ`の値を取る型。
```haskell
ghci> :t LT
LT :: Ordering
```
2つの`Ord`メンバをとる関数`compare`が返す。
```haskell
ghci> 5 `compare` 3
GT
```

### Show
`Show`のメンバは文字列として表すことができる。
型クラス`Show`を扱う、よく使われる関数は`show`である。
`Show`のメンバである値をとり、文字列として表示する。
```haskell
ghci> show 3
"3"
ghci> show 5.334
"5.334"
ghci> show True
"True"
```
### Read
`Read`は`Show`の逆の型クラスのようなものである。
関数`read`は文字列をとり、`Read`のメンバである型の値を返す。
```haskell
ghci> read "True" || False
True
ghci> read "8.2" + 3.8
12.0
ghci> read "5" - 2
3
ghci> read "[1,2,3,4]" ++ [3]
[1,2,3,4,3]
```
なお、`"4"`だけを`read`させようとするとエラーになる。
```haskell
ghci> read "4"
<interactive>:1:0:
    Ambiguous type variable `a' in the constraint:
      `Read a' arising from a use of `read' at <interactive>:1:0-7
    Probable fix: add a type signature that fixes these type variable(s)
```
これは、`read`された値を使っておらず、
どんな値を返せばよいのかわからないためである。

そのため、型アノテーションというものがある。
これは式がどのような型かを明示的に指示する方法である。
`::`を式の末尾につければよい。
```haskell
ghci> read "5" :: Int
5
ghci> read "5" :: Float
5.0
ghci> (read "5" :: Float) * 4
20.0
ghci> read "[1,2,3,4]" :: [Int]
[1,2,3,4]
ghci> read "(3, 'a')" :: (Int, Char)
(3, 'a')
```
基本はコンパイラが推論するが、
このように時々、どんな型を返せばよいのかわからない時がある。
Haskellは静的型付け言語であり、
コンパイル前にすべての型を知っている必要があるため
型を教えてやる必要がある。
### Enum
`Enum`は順序付けされた型であり、つまり列挙可能である。
この型クラスの主要なメリットは、リストのrangeで使うことができる点である。
```haskell
ghci> ['a'..'e']
"abcde"
ghci> [LT .. GT]
[LT,EQ,GT]
ghci> [3 .. 5]
[3,4,5]
ghci> succ 'B'
'C'
```
### Bounded
`Bounded`メンバには上限と下限がある。
```haskell
ghci> minBound :: Int
-2147483648
ghci> maxBound :: Char
'\1114111'
ghci> maxBound :: Bool
True
ghci> minBound :: Bool
False
```
`minBound`と`maxBound`の型は`(Bounded a) => a`であり、
つまりこれらはポリモーフィックな定数である。
### Num
数値型クラス。
```haskell
ghci> :t 20
20 :: (Num t) => t
```
全ての数はポリモーフィックな定数であり、
型クラス`Num`のメンバであるどんな型としてもふるまえる。
```haskell
ghci> 20 :: Int
20
ghci> 20 :: Integer
20
ghci> 20 :: Float
20.0
ghci> 20 :: Double
20.0
```
`Num`のメンバになるには、
すでに`Show`と`Eq`のメンバである必要がある。
### Integral
同じく数値型クラス。
`Num`は実数と整数を含むすべての数を包含するが、
`Integral`はすべての整数を包含する。
この型クラスには`Int`と`Integer`が含まれる。
### Floating
浮動小数点数を含む。
つまり`Float`と`Double`。

## 数を扱う便利な関数`fromIntegral`
以下のコードはエラーになる。
```haskell
length [1,2,3] + 3.2
```
なぜなら、`length`の戻り値が`Int`だからである。
`fromIntegral`は`Integral`をとり、より一般的な数`Num`として返す。
```haskell
ghci> fromIntegral (length [1,2,3]) + 3.2
6.2
```

`fromIntegral`の型は以下のようになっている。
```haskell
ghci> :t fromIntegral
fromIntegral :: (Integral a, Num b) => a -> b
```
なお、複数のクラス制約を持つ場合、このようにコンマで区切る。
