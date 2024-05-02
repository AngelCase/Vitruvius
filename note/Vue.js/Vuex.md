大規模開発に使われる状態管理ライブラリ。
Vuexの全体像：
![[Vuex.png]]
OptionsAPIとの比較：
![[VuexとOptionsAPI.PNG]]

## Vuexの使い方
![[Vuexの使い方.PNG]]
[[Vuex_state|state]]
[[Vuex_mutations|mutations]]
[[Vuex_actions|actions]]
[[Vuex_getters|getters]]
[[Mapヘルパー]]
[[モジュール分割]]

## 設計のコツ
Vuexを使うのはロジックを扱うコンポーネントだけにする。
UIのためのコンポーネントでは使わないようにする。