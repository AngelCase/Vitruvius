GHCupでHaskellツールチェインをインストールして管理する。
ツールチェインには以下が含まれる：
- コンパイラ
- language server
- ビルドツール
エディタはVSCodeにHaskell拡張機能を入れて使う。

## GHCup
GHCupを使えば
Haskellプログラミングに必要なすべてをインストールでき、
それらのアップデートやバージョン切り替えなどを助けてくれる。

ここでインストールする：
[GHCup (haskell.org)](https://www.haskell.org/ghcup/#)
PowerShellで以下のコマンドを実行：
```bash
Set-ExecutionPolicy Bypass -Scope Process -Force;[System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; try { Invoke-Command -ScriptBlock ([ScriptBlock]::Create((Invoke-WebRequest https://www.haskell.org/ghcup/sh/bootstrap-haskell.ps1 -UseBasicParsing))) -ArgumentList $true } catch { Write-Error $_ }
```
何をインストールするかを細かく聞かれるが、
とりあえず規定値に従うことにしたのでEnterを押し続ける。

完了したら、新しいPowerShellで`ghci`を実行すれば、
以下のように対話形式でのインタフェースが開く。
```bash
ghci> 2 + 4
6
```
## 参考
[Get Started (haskell.org)](https://www.haskell.org/get-started/)
