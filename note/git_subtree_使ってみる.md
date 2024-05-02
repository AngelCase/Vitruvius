## 既存のディレクトリをリポジトリとして切り出す
`NagareruHoshinoMure/src/engine`を`split-engine`ブランチに切り出す。
その際、分かりやすさのためにcommitメッセージの頭に`(split)`とつける。
```bash
git subtree split --prefix=NagareruHoshinoMure/src/engine --annotate="(split)" --branch split-engine --rejoin
```
`--rejoin`をつけることで既存ディレクトリのヒストリーにも追加され、次回以降の`split`でそれより古いヒストリーを見なくなるため`split`が早くなる。
※↑のコマンドはリポジトリのルートディレクトリでないと使えない。例えば、`NagareruHoshinoMure/src`から`--prefix=engine`のように指定することはできない。

新しいリポジトリ`engine`を作成する。
```bash
mkdir engine
cd engine
git init
```
元のリポジトリの`split-engine`を新しいリポジトリにpullする。
```bash
git pull ../NagareruHoshinoMure split-engine
```
これで既存のディレクトリをリポジトリとして切り出すことができた。

さらに、GitHubのCLIから見たかったのでpushする。
```bash
git remote add origin git@github.com:AngelCase/engine.git
git push origin main
```

## 切り出したリポジトリを別のリポジトリの一部として利用する

先ほど切り出した`engine`の内容を、別のリポジトリ`Kamishiki`の一部として利用する。
`Kamishiki/src/engine`ディレクトリとして、`engine`リポジトリの`main`の内容を持ってくる。
```bash
git subtree add --prefix=Kamishiki/src/engine ../engine main --squash
```
`--squash`オプションをつけることで持ってくるリポジトリのヒストリーを１つのcommitにまとめられる。
これによってヒストリーが煩雑になるのを防げる。

2回目以降は`pull`で更新できる。
```bash
git subtree pull --prefix=Kamishiki/src/engine ../engine main --squash
```