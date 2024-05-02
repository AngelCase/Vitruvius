## 概要
渡された数が7か否かをチェックする関数は以下のように書ける。
```haskell
lucky :: (Integral a) => a -> String
lucky 7 = "LUCKY NUMBER SEVEN!"
lucky x = "Sorry, you're out of luck, pal!"
```
パターンは上から下に向かってチェックされ、
パターンに適合したら対応する関数が呼ばれる。

`if`でも同じことを実装できるが、
こちらの方がより簡潔に書ける。
（今回は7だけだったが、いろいろな数字にマッチさせようとすると
`if`が長くなる）

階乗も以下のように書ける。
```haskell
factorial :: (Integral a) => a -> a
factorial 0 = 1
factorial n = n * factorial (n - 1)
```

## タプルでのパターンマッチング
2次元空間の2つのベクトルを足す関数を作る。
パターンマッチングを使わない場合は以下のように書ける。
```haskell
addVectors :: (Num a) => (a, a) -> (a, a) -> (a, a)
addVectors a b = (fst a + fst b, snd a + snd b)
```
パターンマッチングを使うと、以下のようにより簡潔に書ける。
```haskell
addVectors :: (Num a) => (a, a) -> (a, a) -> (a, a)
addVectors (x1, y1) (x2, y2) = (x1 + x2, y1 + y2)
```

3つ組から各コンポーネントを取り出す関数は以下のように書ける。
```haskell
first :: (a, b, c) -> a
first (x, _, _) = x

second :: (a, b, c) -> b
second (_, y, _) = y

third :: (a, b, c) -> c
third (_, _, z) = z
```

## リスト内包表記でのパターンマッチング
```haskell
ghci> let xs = [(1,3), (4,3), (2,4), (5,3), (5,6), (3,1)]
ghci> [a+b | (a,b) <- xs]
[4,7,6,8,11,4]
```
パターンマッチに失敗したら、単に次の要素に進む。

## リストでのパターンマッチング
リストにもパターンマッチングを適用できる。
例として`head`関数を自作してみる。
```haskell
head' :: [a] -> a
head' [] = error "Can't call head on an empty list, dummy!"
head' (x:_) = x
```
実行結果：
```haskell
ghci> head' [1] 
1
ghci> head' [1,2,3]
1
ghci> head' []
*** Exception: Can't call head on an empty list, dummy!
CallStack (from HasCallStack):
  error, called at baby.hs:21:12 in main:Main
```
注意として、複数の変数をバインドする場合括弧が必要となる。

また、今回関数`error`を使っている。
`error`は文字列を受け取りランタイムエラーを引き起こす。

### シンタックスシュガーによる書き換え
先頭のいくつかの要素を取り出す簡単な関数を作る。
```haskell
tell :: (Show a) => [a] -> String
tell (x:[]) = show x
tell (x:y:[]) = show x ++ show y
```
これは次のように書き換えることができる。
```haskell
tell [x] = show x
tell [x,y] = show x ++ show y
```
実行結果：
```haskell
ghci> tell [1]
"1"
ghci> tell [1,2]
"12"
```
ただし、要素数が2以上であり確定しない次のような場合だと書き換えはできない。
```haskell
tell (x:y:_) = "too long"
```

## 応用：パターンマッチングと再帰
[[Haskell_リスト内包表記]]の時も作ったが、
関数`length`をパターンマッチングと再帰で自作する。
```haskell
length' :: (Num b) => [a] -> b
length' [] = 0
length' (_:xs) = 1 + length' xs
```
