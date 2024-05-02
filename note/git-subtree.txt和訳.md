## DESCRIPTION
subtreeを使うことで、サブプロジェクトをメインプロジェクトのサブディレクトリに含めることができます。
また、その時サブプロジェクトのヒストリーの全てを含めるかを選べます。

例えば、ライブラリのソースコードをあなたのアプリケーションのサブディレクトリとして含められます。

同じタスクを目的としていますが、subtreeをsubmoduleと混同しないでください。
submoduleと違って、subtreeは`.gitmodules`やgitlinkなどの特別な構造を必要としませんし、
あなたのリポジトリのエンドユーザに特別なことやsubtreeが同機能するかの理解を強制することもありません。
subtreeは単なるサブディレクトリであり、commitでき、ブランチを切ることができ、プロジェクトと一緒にmergeできます。

subtree merge戦略とも混同しないでください。
大きな違いは、他のプロジェクトをサブディレクトリとしてmergeする以外に、
サブディレクトリのヒストリー全体をプロジェクトから抽出しスタンドアローンのプロジェクトにすることができる点です。
subtree merge戦略と違って、これらの2つの操作を行ったり来たりできます。
もしスタンドアローンなライブラリーがアップデートされた場合、
自動的に変更をプロジェクトにmergeできます。
また、もしライブラリをプロジェクト内で更新しても、
その変更を「分割（split）」して再びそのライブラリプロジェクトにmergeできます。

例えば、あるアプリケーションのために作ったライブラリが他の場所でも有用な場合、
アプリケーションプロジェクトのヒストリーと誤って交錯させることなく、
そのヒストリー全体を抽出してそれ自身のgitリポジトリーとしてパブリッシュできます。

\[ヒント\]
commitメッセージをきれいに保つために、subtreeとメインプロジェクトの間でできるだけcommitを分割することをお勧めします。
つまり、もしライブラリとメインアプリケーションの両方に影響する変更を加えたら、2つに分けてcommitします。
そうすることで、ライブラリのcommitを後で切り出してもそれらの説明文は理解可能なままになります。
しかし、これがあなたにとって重要でなければ、_必要ではありません_。
`git subtree`は、サブプロジェクトを切り出す際にライブラリに関係しない部分を単に省きます。

## COMMANDS
### add
```
add <local-commit>::
add <repository> <remote-ref>::
```

与えられた`<local-commit>`か`<repository>`と`<remote-ref>`の内容をインポートして`<prefix>`subtreeを作ります。
新しいcommitが自動的に作られ、インポートされたプロジェクトのヒストリーとあなた自身のものが結合されます。
`--squash`を使うことで、ヒストリー全体ではなく単体のcommitをサブプロジェクトからインポートします。

### merge
```
merge <local-commit> [<repository>]::
```

`<local-commit>`までの最近の変更を`<prefix>`subtreeにmergeする。
通常の`git merge`と同じように、これはあなた自身のローカルの変更を削除せず、単にそれらの変更を最新の`<local-commit>`にmergeします。
`--squash`を使うことで、ヒストリー全体をmergeするのではなく、全ての変更を含む単一のcommitを作成します。

もし`--squash`を使うなら、mergeの方向は常にforwardである必要はなく、このコマンドを例えばv2.5からv2.4への過去へ向かう方向に使うことができます。
もしmergeがconflictを生む場合、いつもの方法で解決できます。

`--squash`を使う場合、また過去の`--squash`によるmergeでsubtreeリポジトリのタグが注釈付きのタグがmergeされた場合、そのタグはローカルで利用可能である必要があります。
`<repository>`が与えられている場合、足りないタグは自動的にそのリポジトリから取得されます。

### split
```
split [<local-commit>] [<repository>]::
```

`<local-commit>`の`<prefix>`subtreeのヒストリーから新しい合成プロジェクトのヒストリーを抽出します。
`<local-commit>`がない場合はHEADのヒストリーから抽出される。
新しいヒストリーには`<prefix>`に影響を与えたcommit（mergeを含む）だけが含まれます。
そして、それらのcommitのそれぞれはサブディレクトリではなくプロジェクトのルートに`<prefix>`の内容として格納されます。
そのため、新しく作られたヒストリーは別のgitリポジトリーとしてエクスポートするのに適しています。

分割が成功した後、単一のcommit IDがstdoutに出力されます。
これは新しく作られたツリーのHEADに対応しており、好きなように操作できます。

まったく同じヒストリーを繰り返し分割しても、`--annotate`のような同じ設定が`split`に渡される限り、同一であることが保証されます（つまり、同じcommit IDを生成します）。
これにより、もし新しいcommitを追加して再度分割したら、新しいcommitは前回作成したヒストリーの先頭のcommitとして追加されるため、`git merge`やその仲間は期待通りに動作します。

以前の`--squash`つきのmergeでsubtreeリポジトリの注釈付きタグがmergeされた場合、そのタグはローカルで利用可能である必要があります。
もし`<repository>`が指定された場合、足りないタグはそのリポジトリから自動的に取得されます。

### pull
```
pull <repository> <remote-ref>::
```

まさに`merge`と似ていますが、指定されたリモートリポジトリから指定された参照を取得するという点で`git pull`と似ています。

### push
```
push <repository> [+][<local-commit>:]<remote-ref>::
```

`<local-commit>`の`<prefix>`subtreeを使って`split`し、
`<repository>`と`<remote-ref>`に`git push`でpushします。
これは、リモートリポジトリの別のブランチにsubtreeをpushするのに使用できます。
`split`同様、`<local-commit>`が指定されていない場合、HEADが使用されます。
オプションの先頭の`+`は無視されます。

## OPTIONS FOR ALL COMMANDS
（省略）

```
-P <prefix>::
--prefix=<prefix>::
```

リポジトリの操作したいsubtreeへのパスを指定します。
このオプションは全てのコマンドで必須です。

## EXAMPLE
### EXAMPLE 1. 'add' command
外部のベンダーのライブラリを追加したいローカルリポジトリがあるとしましょう。
このケースでは、既に存在するgit-extensionsリポジトリのサブディレクトリとして、
git-subtreeリポジトリを`git-extensions`に追加します：

```
$ git subtree add --prefix=git-subtree --squash \
 git://github.com/apenwarr/git-subtree.git master
```

なお、`master`は有効なリモート参照である必要があり、別なブランチ名でも構いません。

また、`--squash`フラグは省略できますが、そうするとローカルリポジトリに含まれるcommitの数が増えます。

これで、git://github.com/apenwarr/git-subtree.gitのmasterブランチのコードを含む
`~/git-extensions/git-subtree`ディレクトリができました。

### EXAMPLE 2. Extract a subtree using 'commit', 'merge' and 'pull'
gitのソースコードのためのリポジトリを例に使ってみましょう。
まず、あなた自身のgit.gitリポジトリを手に入れましょう：

```
$ git clone git://git.kernel.org/pub/scm/git/git.git test-git
$ cd test-git
```

gitweb（commit 1130ef3）はcommit 0a8f4f0としてgitにmergeされ、それ以降もはや別々に管理されていません。
しかし、別々に管理されていたとして、その時からのgitのgitwebへの変更を抜き出し、upstreamと共有したいとします。
このようにすることができます：

```
$ git subtree split --prefix=gitweb --annotate='(split) ' \
  	0a8f4f0^.. --onto=1130ef3 --rejoin \
   	--branch gitweb-latest
$ gitk gitweb-latest
$ git push git@github.com:whatever/gitweb.git gitweb-latest:master
```

（ここで、`0a8f4f0^..`は0a84480を含む0a84480から現在のバージョンまでの全ての変更を意味します）

gitwebがもともと`git subtree add`を使ってmergeされていたら（あるいは`--rejoin`指定で以前のsplitが行われていたら）、
奇妙なcommit IDを覚えることなく、全てのsplitを実行できます：

```
$ git subtree split --prefix=gitweb --annotate='(split) ' --rejoin \
   	--branch gitweb-latest2
```

また、upstreamプロジェクトからの変更を簡単にmergeできます：

```
$ git subtree pull --prefix=gitweb \
  	git@github.com:whatever/gitweb.git master
```

`--squash`を使うことで、初期バージョンのgitwebに巻き戻すことができます：

```
$ git subtree merge --prefix=gitweb --squash gitweb-latest~10
```

いくつかの変更を加えます：

```
$ date >gitweb/myfile
$ git add gitweb/myfile
$ git commit -m 'created myfile'
```

そしてまた早送りします：

```
$ git subtree merge --prefix=gitweb --squash gitweb-latest
```

そしてあなたの変更が無傷であることを知らせます：

```
$ ls -l gitweb/myfile
```

そしてそれを切り出すことができ、標準のgitwebとの違いを確認しましょう：

```
git log gitweb-latest..$(git subtree split --prefix=gitweb)
```

### EXAMPLE 3. Extract a subtree using a branch
大量のファイルとサブディレクトリのソースディレクトリーを持っていると仮定します。
libディレクトリをそれ自身のプロジェクトに切り出したいとしましょう。
これがその近道です：

まず、新しいリポジトリを好きな場所に作ります：

```
$ <go to the new location>
$ git init --bare
```

あなたのオリジナルのディレクトリに戻りましょう：

```
$ git subtree split --prefix=lib --annotate="(split)" -b split
```

新しいブランチを新しい空っぽのリポジトリにpushしましょう：

```
$ git push <new-repo> split:master
```

## オリジナル
[git-subtree.txt](https://github.com/git/git/blob/master/contrib/subtree/git-subtree.txt)