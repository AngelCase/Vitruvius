## 概要
パターンで分解しつつ、全体も同時に束縛する方法として
asパターンがある。

文字列を受け取って
文字列全体と先頭の文字を返す関数を以下のように実装できる。
```haskell
capital :: String -> String
capital "" = "Empty string, whoops!"
capital all@(x:xs) = "The first letter of " ++ all ++ " is " ++ [x]
```
出力結果：
```haskell
ghci> capital "Dracula"
"The first letter of Dracula is D"
```
asパターンを使うことで、`x:xs`を繰り返し書かずに済む。
