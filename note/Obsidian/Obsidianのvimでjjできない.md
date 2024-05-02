## 起こった問題
[Obsidian](https://obsidian.md/)に[vimrcをサポートするプラグイン](https://github.com/esm7/obsidian-vimrc-support)を導入した。
このプラグインを使うことで、Vaultのルートディレクトリに置いた`.obsidian.vimrc`を読み込んでくれるようになる。
しかし、普段vimで使っている「`jj`でインサートモードから抜ける」キーマッピングが効かない：
```
inoremap jj <ESC>
```

## 原因
`.obsidian.vimrc`の書き方に問題があった。
以下のような書き換えが必要：
1. `inoremap`ではなく`imap`を使う
2. `<ESC>`ではなく`<Esc>`と書く

## 解決
`.obsidian.vimrc`に以下のように書くことでキーマッピングが効くようになった：
```
imap jj <Esc>
```

## 参考
https://github.com/esm7/obsidian-vimrc-support/issues/18
