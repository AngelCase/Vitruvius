## 概要
[Rustを使ったWebAssemblyアプリケーション作成のチュートリアル](https://rustwasm.github.io/docs/book/game-of-life/testing.html) を進めていて、
テスト実行のフェーズでエラーが出た。

実行したコマンドは以下：
```sh
wasm-pack test --chrome --headless
```
コマンド実行時のログから、
エラーと関係ありそうな部分を抜粋：
```
...
Running headless tests in Chrome on `http://128.0.0.1:51208/`
Try find `webdriver.json` for configure browser's capabilities: 
Not found
Starting new webdriver session...
DevTools listening on ws://127.0.0.1:51211/devtools/browser/f19b9d6c-9049-4152-be3d-1b6867d9a666
driver status: exit code: 1
driver stdout:
    Starting ChromeDriver 114.0.5735.90 (386bc09e8f4f2e025eddae123f36f6263096ae49-refs/branch-heads/5735@{#1052}) on port 51208 
    Only local connections are allowed.
    Please see https://chromedriver.chromium.org/security-considerations for suggestions on keeping ChromeDriver safe.
    ChromeDriver was started successfully.
...
Error: Running Wasm tests with wasm-bindgen-test failed
Caused by: Running Wasm tests with wasm-bindgen-test failed     
Caused by: failed to execute `cargo test`: exited with exit code: 1
  full command: "cargo" "test" "--target" "wasm32-unknown-unknown"
```

## 原因
`ChromeDriver`と`Chrome`のバージョンに互換性がないため。

##  回避策
ChromeDriverのインストール方法はこちらを参考にした。
[Chrome Driverのインストール方法 (zenn.dev)](https://zenn.dev/ryo427/articles/7ff77a86a2d86a)

適切なバージョンのChromeDriverをインストールする。
[ChromeDriver - WebDriver for Chrome - Downloads (chromium.org)](https://chromedriver.chromium.org/downloads)
ダウンロードしたらPATHの通っているところに配置すること。

##  参考資料
[Testing Life - Rust and WebAssembly (rustwasm.github.io)](https://rustwasm.github.io/docs/book/game-of-life/testing.html)
[Headless Chrome test fails · Issue #611 · rustwasm/wasm-pack · GitHub](https://github.com/rustwasm/wasm-pack/issues/611)
[Chrome Driverのインストール方法 (zenn.dev)](https://zenn.dev/ryo427/articles/7ff77a86a2d86a)
