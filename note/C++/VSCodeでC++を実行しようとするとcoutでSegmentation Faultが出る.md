## 前提
VSCodeでC++を実行するための環境構築はこのページをもとにおこなった。
[Get Started with C++ and MinGW-w64 in Visual Studio Code](https://code.visualstudio.com/docs/cpp/config-mingw#_troubleshooting)

## 発生した問題
以下のコードを実行しようとすると、`cout`の行でsegmentation faultが出る。
```cpp
#include <iostream>
#include <vector>
#include <string>

using namespace std;

int main()
{
	vector<string> msg{"Hello", "C++", "World", "from", "VS Code", "and the C++ extension!"};

	for (const string &word : msg)
	{
		cout << word << " ";
	}
	cout << endl;
}
```
調べたところ、
実行時にロードされるライブラリが想定外のパスからロードされているのが原因だとわかった。
debug consoleのログによると以下のパスからロードされているが、
```
Loaded 'C:\Program Files\Git\mingw64\bin\libstdc++-6.dll'. Symbols loaded.
```
[stack overflow](https://stackoverflow.com/questions/76631846/c-segmentation-fault-on-hello-world-cin-and-cout)によると正しくはこう。
```
Loaded 'C:\msys64\mingw64\bin\libstdc++-6.dll'. Symbols loaded.
```
おそらく、libstdc++-6.dllを想定外のパスからロードしていることで、
名前は同じだがバージョン違いのDLLを読み込んでしまい
不具合が出ている。

このような意図しないパスでDLLが呼ばれる現象は[DLL地獄](https://en.wikipedia.org/wiki/DLL_Hell)と呼ばれる典型的な現象らしい。

## 解決策1
人によっては、Git側のディレクトリ名を一度変更してコンパイル、
そのあとディレクトリ名を戻せば以降は問題なく動いたという報告もあった。
```
`C:\Programs\Git\mingw64`
↓
`C:\Programs\Git\mingw64ABC`（この時点で一度コンパイル）
↓
`C:\Programs\Git\mingw64`
```
[visual studio code - Simple hello world segfaults in VSCode Run C/C++ file - Stack Overflow](https://stackoverflow.com/questions/76972637/simple-hello-world-segfaults-in-vscode-run-c-c-file)
しかし、自分の環境だとこの方法では解決しなかった。

## 解決策2
コンパイル時に生成される`.vscode/tasks.json`に以下の記述を追記することで解決した。
```json
"args": [
	...略...
	"-static-libstdc++"
],
```
### なぜこれで解決するのか
推測だが、
libstdc++6.dllの「動的ライブラリ」ではなく「静的ライブラリ」を使うことで
DLLの問題を回避しているのだと思われる。

## その他
今回のcoutでsegmentation faultが出る現象は`-static-libstdc++`の追加だけで解消されたが、
libgcc_s_seh-1.dllも同様にGitディレクトリからロードされていた。
そちらで問題が起きた場合、同じように`"-static-libgcc"`と追加することで解決するかもしれない。

## 参考
[[MinGW] libstdc++-6.dllが見つからないため… | Tech控え帳 (chihayafuru.jp)](https://www.chihayafuru.jp/tech/index.php/archives/2980)
