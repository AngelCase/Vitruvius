## 概要
Nuxt3で作ったポートフォリオをGitHub Pagesで公開した。
[ポートフォリオ](https://angelcase.github.io/portfolio/)
[リポジトリ](https://github.com/AngelCase/portfolio)
ついでにGitHub Actionsで以下のようなデプロイの流れを構築したので、本記事ではその手順について説明する。
![[ポートフォリオのブランチ戦略.png]]

## SSRかSSGか
今回作るのはポートフォリオ。つまりユーザごとに違う画面を表示したりしない。
そのためSSGを選択する。
また、SSGを選択するため、`npm run generate`で静的サイトを生成する。
### 参考
[【Nuxt.js】buildとgenerateの違い | mktia's note](https://blog.mktia.com/diffrences-between-build-and-generate-in-nuxt/)
[SPA, SSR, SSGって結局なんなんだっけ？ (zenn.dev)](https://zenn.dev/rinda_1994/articles/e6d8e3150b312d)
[SSR、SSG、Client Side Renderingの違いをまとめた - Qiita](https://qiita.com/akashixi/items/84cd79e090a283bb8c67)

## Nuxt3側の設定
`nuxt generate`の生成先ディレクトリとベースディレクトリを設定する。
nuxt.config.tsに以下のように記述すればよい：
```js
// https://nuxt.com/docs/api/configuration/nuxt-config
export default defineNuxtConfig({
	nitro: {
		output: {
			publicDir: './docs'
		}
	},	
	app: {
		baseURL: '/portfollio/'
	}
})
```
`portfolio`の部分はリポジトリ名。

### ハマった部分
`nuxt generate`の生成先ディレクトリを指定する際、検索してもNuxt2の情報しか出てこず困った。以下のような記述はNuxt3だとできない。
```js
generate: {
	dir: 'build/_site'
}
```
最終的にたどり着いた[stack overflow](https://stackoverflow.com/questions/75794580/nuxt-3-nuxt-generate-change-static-output-directory)によるとnitroのconfigを上書きしてやればよいらしい。
ルートディレクトリ直下の`/docs`内に生成ファイルを置いてほしいので以下のように設定している。
```javascript
export default defineNuxtConfig({
  ...
	nitro: {
		output: {
			publicDir: '../docs'
		}
	},	
  ...
}
```
なお、`../`をつけないと`/server/docs`に配置される。

ここまでの設定で、`yarn run generate`を実行すると`/docs`にビルド結果が生成されるようになった🎉

### 参考
[nitroの公式ドキュメント](https://nitro.unjs.io/config/)
[Output dir config · unjs/nitro · Discussion #1066 (github.com)](https://github.com/unjs/nitro/discussions/1066)

## GitHub側の設定
今回は以下のようなデプロイの流れを構築する（再掲）。
![[ポートフォリオのブランチ戦略.png]]
`production`ブランチにmergeしたら、`gh-pages`ブランチにデプロイされるようにする。
なお、`production`ブランチは試行錯誤で理解を深めるために作ったので、不要であれば`master`へのpushをトリガに直接`gh-pages`へとmergeすればよい。
まずは`production`と`gh-pages`ブランチを切る。
![[productionブランチを作成.PNG]]

### productionにpush → gh-pagesにデプロイ
GitHub Actionsを使う。
まずActionsのWorkflowに書き込み権限を渡してやる必要がある。
リポジトリのSettings→Actions→General→下の方のWorkflow permissionsでRead and write permissionsを選択、Saveで設定を保存。
![[github_workflow_permissions.PNG]]
参考：[GitHub ActionsでGitHub PagesにデプロイしようとしたらPermissionのエラーが出た | Tech Blog - Akihiro Suzuki](https://akihiro.dev/entries/github-pages-deploy-actions-permissions/)

次に以下のようなyamlを`/.github/workflows/`内に配置する：
```yaml
name: Build and Deploy
on:
  push:
    branches:
      - production
jobs:
  build-and-deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout 🛎️
        uses: actions/checkout@v3

      - name: Build 🔧
        run: |
          yarn install
          yarn run generate

      - name: Deploy 🚀
        uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./docs
```
これで`production`ブランチにpushすれば自動で`gh-pages`にデプロイされるようになった🎉

以下、個人的に気になった部分について。

#### checkout？
workflowにてアクションなどをソースコードに適用するには、「ソースコードをcheckoutする」という工程を挟む必要があるらしい。
[GitHub Actions を理解する - GitHub Docs](https://docs.github.com/ja/actions/learn-github-actions/understanding-github-actions)

#### GITHUB_TOKEN？
これは別にpersonal access tokenではないらしい。
[peaceiris/actions-gh-pages](https://github.com/peaceiris/actions-gh-pages)のREADME.mdに説明がある：
>For newbies of GitHub Actions: Note that the `GITHUB_TOKEN` is **NOT** a personal access token. A GitHub Actions runner automatically creates a `GITHUB_TOKEN` secret to authenticate in your workflow. So, you can start to deploy immediately without any configuration.

GitHub Actionsのランナーが自動的に生成するので、特に何かtokenを用意してやる必要はないとのこと。

### gh-pagesにpush → GitHub Pagesにデプロイ
まずリポジトリをpublicにする。あるいはアカウントをアップグレードする。
次にSetting→Pages→Build and deployment→Branchで`gh-pages`、`/(root)`を指定してSave。
![[github_pages_dir.PNG]]
これでデプロイされたページが開けるようになる。
Pages設定の上の方にVisit siteというボタンがあるのでそこからデプロイされたページにアクセスできる。

## 全体の手順を考えるうえで参考にしたサイト
[Nuxt.jsで静的サイトを作ってGitHub Pagesにデプロイするまで : 勉強の履歴 (livedoor.jp)](http://blog.livedoor.jp/zither_side/archives/15350745.html)
[Nuxt.jsで作成したサイトをGithub Pagesに公開する (Github Actionsによるデプロイ自動化あり) - Qiita](https://qiita.com/Ancient_Scapes/items/fe18bae043e4d35f1e39)
上のサイトを参考にしていたが、Nuxt2と3で微妙に設定が違ったため以下を参考にした：
[router.baseを設定したNuxt2をNuxt3へマイグレーションする際の方法(app.baseURL) - Qiita](https://qiita.com/rmlabo/items/10da7b158fda5e1f48c9)
