main.jsがエントリーポイントであり、最初に読み込まれるファイル。

node_modules: 各種ライブラリ
dist: ビルド時に生成されるファイル群
public: HTMLファイル
src: ソースコード
package.json: npmの設定ファイル（インストールされるファイルの情報）
vue.config.js: Vueの設定ファイル

## 他環境での実施
node_modulesやdistは必要ないのでリポジトリにも含めない。