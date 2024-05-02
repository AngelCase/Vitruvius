## 概要
ガードはある値が真か偽かを判定する、if文のような役割を果たす。

以下はガードを使ったBMIを計算する関数の例：
```haskell
bmiTell :: (RealFloat a) => a -> String
bmiTell bmi
    | bmi <= 18.5 = "You're underweight, you emo, you!"
    | bmi <= 25.0 = "You're supposedly normal. Pffft, I bet you're ugly!"
    | bmi <= 30.0 = "You're fat! Lose some weight, fatty!"
    | otherwise   = "You're a whale, congratulations!"
```
booleanの式がtrueなら対応する関数のボディが使用される。
falseなら下に降りていく。

多くの場合最後のガードは`otherwise`である。
これはtrueになり、すべてをキャッチする。
もし`otherwise`がなく、すべてfalseになったらエラーが投げられる。

## 例：複数パラメータをとる
複数パラメータを取り、それを計算できる。
```haskell
bmiTell :: (RealFloat a) => a -> a -> String
bmiTell weight height
    | weight / height ^ 2 <= 18.5 = "You're underweight, you emo, you!"
    | weight / height ^ 2 <= 25.0 = "You're supposedly normal. Pffft, I bet you're ugly!"
    | weight / height ^ 2 <= 30.0 = "You're fat! Lose some weight, fatty!"
    | otherwise                 = "You're a whale, congratulations!"
```

## 例：1行で書く
`max`関数を自作する。
```haskell
max' :: (Ord a) => a -> a -> a
max' a b 
    | a > b     = a
    | otherwise = b
```

可読性は下がるが、以下のように1行で書くこともできる。
```haskell
max' :: (Ord a) => a -> a -> a
max' a b | a > b = a | otherwise = b
```
