## submodule

### 概要
あるリポジトリを別のリポジトリのサブディレクトリとして扱うための機能。
親リポジトリは子リポジトリの特定のコミットを参照する。

### 問題点
- 子リポジトリの差分がコミットハッシュとして見えるため、差分がわかりにくい。
- やりたいことは 1 つなのに、2 か所でコミットが必要になることがある。
- 複数ブランチを切って開発していると、意図せず古い参照をコミットしてしまうことがある。
- 子リポジトリを変更しておきながら親リポジトリだけをコミットするということができるため、複数人での開発で混乱を招きやすい。

## subtree
### 概要
submodule とは違い、子リポジトリのコード自体が親リポジトリの管理下に入る。

### 問題点
- 子リポジトリ側のコミット履歴が親のヒストリーにも入ってくる。
  - `--squash`オプションを付けることで、1 つのコミットにまとめられる。

### コマンド
#### add
他のリポジトリの内容を現在のリポジトリの一部として統合する。
`--squash`でコミットを1つにまとめられる。

#### merge
他のリポジトリの変更を現在のリポジトリにmergeする。
`--squash`でコミットを1つにまとめられる。

#### split
指定したディレクトリに影響を与えたcommitのヒストリーを抽出する。
簡単に言うと、特定のディレクトリの内容を別のリポジトリとして切り出す。
`--rejoin`で切り出したヒストリーを元のリポジトリにmergeする。
`--rejoin`でsplitしておけば、次回以降のsplitの時、過去のsplitの前のヒストリーを検索対象外にしてくれる（処理が早くなる）。

#### pull
他のリポジトリの変更を取得してきて、現在のリポジトリにmergeする。

#### push
指定したディレクトリをsplitし、指定のリポジトリにpushする。

### git-subtree.txt 和訳
ウェブサイトを色々漁ったがよくわからなかったので一次ソースにあたる。
[[git-subtree.txt和訳]]

### よくわからんので使ってみる
[[git_subtree_使ってみる]]

## 参考
[Git subtreeによるライブラリ管理について](http://space-on-my-hands.blogspot.com/2014/09/git-subtree.html)
[git submodule と git subtree から見る外部リポジトリの取り扱い - tech.guitarrapc.cóm](https://tech.guitarrapc.com/entry/2019/03/16/065700)
[git-subtree.txt](https://github.com/git/git/blob/master/contrib/subtree/git-subtree.txt)
https://coffee-nominagara.com/git-subtree-memo
