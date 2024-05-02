API用のクエリ言語であり、
既存のデータを使用してこれらのクエリを実行するためのランタイム。

## RESTとの違い
RESTは以下のような問題点を抱えているが、
GraphQLはそれを克服しようとしている。
- リクエストの構造が固定なので必ずしも効率的でない
- 必要なデータが1つだけでも、全部のデータセットを受け取ることになる
- 複数データが欲しい場合、それぞれのAPIにリクエストが必要になる

### リクエストとレスポンス
必要なものだけ指定して帰ってくる様子を例に示す。
例えば、RESTの`GET /posts`は以下を返す。
```json
[
  {
    "id": 1,
    "title": "First Post",
    "content": "This is the content of the first post."
  },
  {
    "id": 2,
    "title": "Second Post",
    "content": "This is the content of the second post."
  },
  {
    "id": 3,
    "title": "Third Post",
    "content": "This is the content of the third post."
  }
]
```
GraphQLの`GET /graphql?query{post(id: 1) {id title content}}`は以下を返す。
```json
{
  "data": {
    "posts": [
      {
        "id": "1",
        "title": "First Post",
        "content": "This is the content of the first post."
      },
]}}
```
### バージョニング
RESTは以下のようにURLにバージョニングを含める。
`https://example.com/api/v1/person/12341`
GraphQLだと、下位互換性を維持する必要がある。

### GraphQLの方が適している場面
- 帯域幅に制限があり、リクエストとレスポンスの数を最小限に抑えたい
- 複数のデータソースがあり、それらを 1 つのエンドポイントにまとめたい
- クライアントのリクエストが大きく異なり、求められるレスポンスも大きく異なる

### RESTの方が適している場面
- アプリケーションの規模が小さく、データがそれほど複雑ではない
- すべてのクライアントが同じように使用するデータと操作がある
- 複雑なデータクエリの必要がない

## セキュリティ課題
クライアント側でレスポンス内容を決められるため、
簡単に負荷の高いクエリを投げられてしまう。

Query depthやQuery complexityによる制限を設けることで防ぐことができるが、
適切な閾値の設定は難しい面もあるため、
徐々に最適化していく必要がある。

## 参考
[【徹底解説】REST VS GraphQL (zenn.dev)](https://zenn.dev/nameless_sn/articles/the_differences_between_rest_and_gql)
[GraphQL と REST API の比較 - API デザインアーキテクチャの違い - AWS (amazon.com)](https://aws.amazon.com/jp/compare/the-difference-between-graphql-and-rest/)
[GraphQLを導入する時に考えておいたほうが良いこと | メルカリエンジニアリング (mercari.com)](https://engineering.mercari.com/blog/entry/20220303-concerns-with-using-graphql/)