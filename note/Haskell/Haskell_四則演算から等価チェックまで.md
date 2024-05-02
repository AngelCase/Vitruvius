マイナスの値を使う場合括弧で囲う必要がある（でないとGHCIに怒られる）。
```haskell
ghci> 5 * (-3)
-15
```
ブール代数は普通。`&&`、`||`、`not`が使える。
```haskell
ghci> True && False
False
ghci> True && True
True
ghci> False || True
True 
ghci> not False
True
ghci> not (True && True)
False
```
等価性の確認：
```haskell
ghci> 5 == 5
True
ghci> 1 == 0
False
ghci> 5 /= 5
False
ghci> 5 /= 4
True
ghci> "hello" == "hello"
True
```
なお、ここまでに出てきた`*`は関数である（infix function）。
