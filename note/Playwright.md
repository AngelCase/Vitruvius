LibraryとTestの2種類がある。
Libraryはブラウザの起動から記述しなければならないのに対して、
Testの方は名前の通りテスト向きで、テスト以外のことはほとんど考えずにコーディングできる。
[Library | Playwright](https://playwright.dev/docs/library)

今回はブラウザの自動操作が目的でテストはいらない。
	なのでLibraryの方をインストールする。
```sh
yarn add --dev playwright
```