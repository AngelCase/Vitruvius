## 基本
[[Single_Page_Application|SPA]]の実現に必要。
router-linkとrouter-viewというタグが追加される。
router-linkでリンクを画面に表示する：
```
<router-link to="/foo">Go to Foo</router-link>
```
router-viewでルートアウトレットを配置する。
ルートとマッチしたコンポーネントがここに描画される：
```
<router-view></router-view>
```

## その他コンテンツ
[[$routerと$route]]
[[Vue_Routerで同じページで表示を切り替えるとライフサイクルが呼ばれない|同じページで表示を切り替えるとライフサイクルが呼ばれない]]
[[Vue_Routerで404処理|404処理]]
[[ネストされたルート]]
[[名前付きビュー]]
[[トランジションつきrouter-view]]
[[ナビゲーションガード]]
### historyモードの注意
ビルドしてdistに吐き出す際には、ベースとなるページを自分で設定する必要がある。