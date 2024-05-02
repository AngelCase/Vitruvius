## 追記
公式でやり方が出ている：
[Siv3D v0.6.14 新規追加・更新サンプル (zenn.dev)](https://zenn.dev/reputeless/scraps/4d973fd3bd10b0#comment-fd6585920f0136)

## 手順
インストーラをダウンロードして実行。
[Siv3D](https://siv3d.github.io/ja-jp/)

既存のプロジェクトにOldなど付けリネームする。
新バージョンのSiv3Dでプロジェクトを作成する。

元のプロジェクトから以下を除くすべてのファイルを持ってくる。
- `*.vcxproj`
- `*.vcxproj.user`
- `*.vcxproj.filters`
- `*.sln`

.vcxprojは`<ItemGroup>`内を、元のプロジェクトを維持する形で持ってくる。
VSCodeでGitの差分を見ながらやるとやりやすい。

## 過去作業時メモ
### v0.6.1 → v0.6.3
2022/04/03
NagareruHoshinoMure.vcxprojをいじくってバージョンアップしようと思ったけど、
エンジン内のフォントの追加があったせいでできなかった。
GUIDとかいう世界で一意な番号がいちいちすべてのリソースに振られてるせい。
だから、プロジェクトを新規作成して、そこにコードを移す形にした。

ただしこの方法をとるとソリューションエクスプローラー内のフィルタとかの情報が消えるので注意。
NagareruHoshinoMure.vcxproj.filtersにソリューションエクスプローラーのフィルター情報が入ってるので、
そっちをdiff見ながら変更していくべし。
