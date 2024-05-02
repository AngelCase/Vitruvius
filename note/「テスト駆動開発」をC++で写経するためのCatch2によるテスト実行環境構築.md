## はじめに
[テスト駆動開発](https://www.amazon.co.jp/%E3%83%86%E3%82%B9%E3%83%88%E9%A7%86%E5%8B%95%E9%96%8B%E7%99%BA-Kent-Beck/dp/4274217884)の写経をC++でやりたい。
そのため、今回は以下のようなテスト環境の構築方法について説明する。
- [Catch2](https://github.com/catchorg/Catch2)を使ってテストを書く
  - 選定理由：個人的に使っている[OpenSiv3D](https://siv3d.github.io/ja-jp/)というフレームワークで使われてて自分も使ってみたくなった
- Visual Studio上でテストを実行する

最終的にこんな感じ：[リポジトリ](https://github.com/AngelCase/cpp-tdd/tree/qiita)

## プロジェクトの作成
テスト対象のプロジェクト`cpp-tdd`もテストを書くプロジェクト`test`も、同じソリューションの中にコンソールアプリとして作る。
![[visualstudio_新しいプロジェクト.PNG]]
こんな感じの構成になる。
![[visualstudio_テストプロジェクト_ソリューションエクスプローラー.PNG]]

## Catch2のヘッダーを配置する
Catch2の現時点（2023/5/3）最新バージョンはv3だが、シングルヘッダーのライブラリを使いたいのでv2を使う。
デフォルトブランチである`devel`ブランチは最新バージョン（v3）なので、`v2.x`ブランチに切り替えて[catch.hpp](https://raw.githubusercontent.com/catchorg/Catch2/v2.x/single_include/catch2/catch.hpp)をとってくる。
これをテストプロジェクトのディレクトリ配下に配置。

## テストプロジェクトの下準備
以下のコードだけ書かれた`Test.cpp`を用意する。
```c++:Test.cpp
#define CATCH_CONFIG_MAIN
#include "catch.hpp"
```
これで`main()`を自動的に作ってくれる。
自動生成される`main()`では、テスト実行時のコマンドライン引数を受け取る、みたいなことをやってくれてるみたい（[参考](https://github.com/catchorg/Catch2/blob/v2.x/docs/tutorial.md#what-did-we-do-here)）。

## 実際にテストを書く
「テスト駆動開発」の第1章を参考に書いていく。
テスト対象のクラス`Dollar`：
```c++:cpp-tdd/Dollar.hpp
#pragma once

namespace money {
	class Dollar {
	public:
		int amount;
		Dollar(int amount) {
			this->amount = amount;
		}
		void times(int multiplier) {
			amount *= multiplier;
		}
	};
}

```
テストを実行するコード：
```c++:test/MoneyTest.cpp
#include "catch.hpp"
#include "../cpp-tdd/Dollar.hpp"

namespace money_test {
	using money::Dollar;

	TEST_CASE("multiplication") {
		auto five = std::make_unique<Dollar>(5);
		five->times(2);
		REQUIRE(five->amount == 10);
	}
}
```

## F5でテストプロジェクトが実行されるようにする
ソリューションエクスプローラーからテストプロジェクトを右クリック、「スタートアッププロジェクトに設定」。
これでF5でテストを実行できる。
![[catch2_pass.PNG]]
これでテスト環境はできあがり！

## おまけ：includeを楽に書く
テスト対象ファイルを`include`するとき、パスの指定がめんどくさい。
```c++
#include "../cpp-tdd/Dollar.hpp"
```
以下のように書きたい。
```c++
#include "Dollar.hpp"
```
手順は以下の通り。
ソリューションエクスプローラーからテストプロジェクトを右クリック、プロパティを開く。
「VC++ ディレクトリ」→「インクルードディレクトリ」、プルダウンから「編集」、「新しい行を追加」で`$(SolutionDir)/cpp-tdd`を追加する。
![[visualstudio_インクルードディレクトリ.PNG]]
これでソリューションのルートにある`cpp-tdd`ディレクトリ直下から直接`include`できる。

## まとめ
Catch2でテストを書くための環境構築の手順について説明した。
最終的なファイル構成はこちら：[リポジトリ](https://github.com/AngelCase/cpp-tdd/tree/qiita)

## 参考
[Visual Studio 2019でCatchを使ったC++単体テストを実現する - Gaming Life (hatenablog.com)](https://ai-gaminglife.hatenablog.com/entry/2020/03/29/110651)
[C++テンプレートの学習にstatic_assertだけで『テスト駆動開発』多国通貨を写経する - Qiita](https://qiita.com/tomoyuki-nakabayashi/items/f3ebf940967561a752c8)
[VisualStudioのincludeファイルパス設定について - Qiita](https://qiita.com/akurobit/items/b6c05723c407b6bfd7f1)