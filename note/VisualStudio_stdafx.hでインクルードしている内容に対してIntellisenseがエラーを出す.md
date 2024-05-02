## 環境
Visual Studio Community 2022 (64ビット)
## 問題

コンパイルは通るのにIntellisenseがエラーと判断して赤線が消えない。

![[VisualStudio_stdafx_赤線.png]]
マウスオーバーすると以下のエラー内容が出る。
```
識別子"String"が定義されていません
```

エラーが出ているのはstdafx.hで定義しているはずの内容で、なぜかIntellisenseはそれらを「定義されていない」と判断している。

## 原因
stdafx.hをプロジェクトのルートディレクトリに置いており、この場所は自分の環境だとIntellisenseが効いていなかった。
```
プロジェクトのルート
  ├── src（自分が主にソースコードを置いている場所）
  ├── stdafx.cpp
  └── stdafx.h
```

## 解決
自分の場合、プロジェクト→「プロジェクト名」のプロパティ→構成を「すべての構成」にして構成プロパティ→C/C++→全般→追加のインクルードディレクトリに`$(ProjectDir)`を追加することで解決した。
![[VisualStudio_stdafx_追加のインクルードディレクトリ.png]]

## 参考
[MS Visual Studio 2022 intellisense stdafx.h problem - Microsoft Q&A](https://learn.microsoft.com/en-us/answers/questions/1139242/ms-visual-studio-2022-intellisense-stdafx-h-proble)
[インテリセンスが効かない場合の対処方法（おいらの場合） - ir9Ex’s diary (hatenablog.jp)](https://ir9ex.hatenablog.jp/entry/20090517/1242566846)
