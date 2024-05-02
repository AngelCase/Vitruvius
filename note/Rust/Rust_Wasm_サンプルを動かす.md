今回はWebAssemblyのサンプルを動かす。
プロジェクト名は`wasm-game-of-life`とする。
```sh
$ cargo generate --git https://github.com/rustwasm/wasm-pack-template
Project Name: wasm-game-of-life
```

## 中身
```sh
wasm-game-of-life/
├── Cargo.toml
├── LICENSE_APACHE
├── LICENSE_MIT
├── README.md
└── src
    ├── lib.rs
    └── utils.rs
```
`Cargo.toml`には、あらかじめ`wasm-bindgen`の依存関係が記述されている。

`src/lib.rs`はWebAssemblyにコンパイルするRustクレートのルート。
`wasm-bindgen`でJavaScriptとインタフェースしている。

`src/utils.rs`はWebAssemblyにコンパイルされたRustの操作を簡単にするための共通ユーティリティ。

## プロジェクトをビルドする
プロジェクトのディレクトリ内でこのコマンドを実行することで、
ビルドが実行される。
```sh
wasm-pack build
```
具体的には以下のステップが行われている：
- Rustが1.30以上であり、`rustup`によって`wasm32-unknown-unknown`ターゲットがインストールされていることの確認
- Rustのソースを`cargo`で`.wasm`バイナリにコンパイル
- Rustで作られたWebAssemblyを使うためのJavaScript APIを`wasm-bindgen`で生成

ビルドが完了したら以下が生成される。
```
pkg/
├── package.json
├── README.md （メインプロジェクトからコピーされる）
├── wasm_game_of_life_bg.wasm
├── wasm_game_of_life.d.ts
└── wasm_game_of_life.js
```
- package.json：JavaScriptとWebAssemblyのパッケージについてのメタデータ
- wasm_game_of_life_bg.wasm：WebAssemblyバイナリ
- wasm_game_of_life.d.ts：TypeScriptを使う場合の型定義
- wasm_game_of_life.js：RustにDOMとJavaScript関数をインポートし、JavaScriptにWebAssembly関数のAPIを公開するためのJavaScriptグルー

## Webページに配置する
`wasm-game-of-life`をWebページに置くために、
`create-wasm-app`というJavaScriptのプロジェクトテンプレートを使う。

以下のコマンドを実行する。
```sh
npm init wasm-app www
```
以下が生成される。
```sh
wasm-game-of-life/www/
├── bootstrap.js
├── index.html
├── index.js
├── LICENSE-APACHE
├── LICENSE-MIT
├── package.json
├── README.md
└── webpack.config.js
```
- package.json：`webpack`と`webpack-dev-server`の依存関係に加え、初期の`wasm-pack-template`パッケージのバージョンである`hello-wasm-pack`への依存関係が事前に設定されている
- webpack.config.js：webpackとローカル開発サーバの設定
- index.html：ルートのHTML
- index.js：WebページのJavaScriptのメインのエントリーポイント
- bootstrap.js：`index.js`のラッパー

## 依存関係のインストール
ローカル開発サーバとその依存関係をインストールするため、
`wasm-game-of-life/www`で、以下のコマンドを実行する。
```sh
npm install
```
注意：
webpackは今回便宜上選んだだけで、必ずしも必要ではない。

## `wasm-game-of-life`パッケージを`www`で使う
現状、`wasm-game-of-life`ではなく`hello-wasm-pack`が使われている状態なので、
`wasm-game-of-life`を使うように変更する。

`www/package.json`を開き、`devDependencies`の隣に`dependencies`として`wasm-game-of-life`を追加する。
```json
{
  // ...
  "dependencies": {                     // Add this three lines block!
    "wasm-game-of-life": "file:../pkg"
  },
  "devDependencies": {
    //...
  }
}
```
次に、`www/index.js`を変更して`wasm-game-of-life`をimportする形に変更する。
```js
import * as wasm from "wasm-game-of-life";

wasm.greet();
```
最後に、依存関係を変更したので再度インストールする。
```sh
npm install
```
これでローカルサーバをたちあげる準備ができた。

## ローカルサーバ起動
`www`ディレクトリで、以下のコマンドを実行する。
```sh
npm run start
```
localhost:8080をブラウザで開けば、期待通りのアラートメッセージが出る。
![[hello_wasm.png]]

変更を加えたら、`wasm-pack build`を`wasm-game-of-life`ディレクトリで再実行すれば反映される。
