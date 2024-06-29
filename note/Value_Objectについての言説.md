## Value Objectとは
1. 比較がIDではなく値によって行われるオブジェクト
2. コピーと参照の違いがあいまいな言語において、不変性の強制や常にコピーするなどの工夫がある
3. ValueのようにふるまうためValue Objectと呼ぶ
2と3に関しては、Cなどの言語だと恩恵は少ない。

「ValueをObjectとして扱う（値にロジックを集めてロジックにする）」ではなく、
「ObjectをValueとして扱う（オブジェクトを値のように使う）」が出発点。

## よくある誤解：全部Value Objectにすべき
そもそも値であるプリミティブ型をわざわざクラスで包んで
コピーではなく参照されるという問題を起こし、
それをイミュータブルなデザインで解決する、という流れはマッチポンプ的で不毛。

## よくある誤解：凝集性が高まる
「Value Objectにすることによって凝集性が高まる」という考えには注意すべきである。
例えば、日付として「発送日」「契約日」「支払日」など独立クラスを定義したとて、
「存在しない日付は作れない」「未来の日付は持てない」などの
原始的なバリデーションにしかならない。
実際の業務では発送日と契約日を組み合わせた制約が出てくるはずで、
結局それらのロジックが散らばるという点で高凝集にならない。
## 参考
[73. Value Object w/ kumagi | fukabori.fm](https://fukabori.fm/episode/73)
[Value Objectについて整理しよう - Software Transactional Memo (hatenablog.com)](https://kumagi.hatenablog.com/entry/value-object)
[#fukabori をきいて Value Object と Value Object パターンについて頭の中を整理 - Mitsuyuki.Shiiba (hatenablog.com)](https://bufferings.hatenablog.com/entry/2022/05/17/010943)
