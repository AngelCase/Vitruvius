リモートプロシージャコール（RPC）モデルに基づくAPIアーキテクチャ。

## RPCとは
ネットワーク上で接続された
他のコンピュータのプログラムを呼び出して実行する技術。
日本語で「遠隔手続き呼び出し」と訳される。

## gRPCとRPC
gRPCはRPCの1つの実装である。
データ通信にHTTP 2とプロトコルバッファ（後述）を使用する。
また、gRPCは基盤となるHTTP通信を抽象化しているため、
開発者はHTTPを意識することなく開発できる。

## プロトコルバッファ
プロトコルバッファとは、
Googleによってつくられた、構造化データをシリアル化するためのメカニズム。
JSONに似ているが、より軽量で高速。
以下の`.proto`ファイルのような定義言語を使う。
```protobuf
message Person {
  optional string name = 1;
  optional int32 id = 2;
  optional string email = 3;
}
```
このデータ構造から、
protoコンパイラ`protoc`は
様々なプログラミング言語のためのデータアクセスクラスのコードを生成する。
（例えば`name()`や`set_name()`などのアクセサー、
また生のバイトとの間でのシリアライズ/解析を行うメソッド）
以下は生成されたメソッドをJavaで使用している例。
```java
Person john = Person.newBuilder()
    .setId(1234)
    .setName("John Doe")
    .setEmail("jdoe@example.com")
    .build();
output = new FileOutputStream(args[0]);
john.writeTo(output);
```

gRPCサービスは以下のように、
RPCメソッドパラメータと返り値の型を
プロトコルバッファのメッセージで定義する：
```protobuf
// The greeter service definition.
service Greeter {
  // Sends a greeting
  rpc SayHello (HelloRequest) returns (HelloReply) {}
}

// The request message containing the user's name.
message HelloRequest {
  string name = 1;
}

// The response message containing the greetings
message HelloReply {
  string message = 1;
}
```

## RESTとの違い
### コミュニケーションモデル
RESTの場合、
クライアントが1つのリクエストをサーバーに送信し、
サーバーは返答として1つの応答を送信する。
このメカニズムはリクエスト/レスポンスモデルであり、単項データ接続（1対1）である。

これとは対照的に、
gRPCの場合
クライアントは1つ以上のAPIリクエストをサーバーに送信でき、
サーバーは1つ以上の応答を返す。
そのため、以下のようなデータ接続がある。
- 単項（1対1）
- サーバーストリーミング（1対多数）
- クライアントストリーミング（多数対1）
- 双方向ストリーミング（多数対多数）
このメカニズムはクライアント/レスポンス通信モデルであり、
gRPCがHTTP 2に基づいているため可能となっている。

### サーバー上で呼び出せる操作
gRPC APIでは
アプリケーション内で内部的に関数を呼び出すのと同じように、
クライアントはサーバー上の関数を呼び出す（サービス指向設計）。
```
createNewOrder(customer_id, item_id, item_quantity) -> order_id
```

RESTでは
クライアントはリソース自体を呼び出す（エンティティ指向設計）。
```
POST /orders <headers> (customer_id, item_id, item_quantity) -> order_id
```

### データ交換形式
RESTでは通常JSONデータ交換形式で表現される。
（XMLやHTMLも可能）
JSONは読みやすく柔軟だが、シリアル化してプログラミング言語に翻訳する必要がある。

対照的に、gRPCではデフォルトでプロトコルバッファ（Protobuf）形式を使用する。
（JSONも可能）
サーバーはprotoファイルのインターフェイス記述言語（IDL）を使用してデータ構造を定義する。
次にgRPCはその構造をシリアル化し、任意のプログラミング言語に逆シリアル化する。
このメカニズムにより、送信中に圧縮されないJSONよりも高速になる。
プロトコルバッファはJSONとは違い人間が読める形式ではない。

### クライアントとサーバーの結合
RESTはクライアントとサーバーは疎結合だが、
gRPCは緊密に結合している。
クライアントとサーバーは同じprotoファイルにアクセスできる必要があり、
ファイル更新にはクライアントとサーバー両方で更新が必要。

### コードの生成
gRPCには、ネイティブコード作成機能があり、
プロトコルバッファコンパイラのprotocで複数言語で利用できる。

RESTは組み込みのコード生成メカニズムがないため
追加のサードパーティーツールを使用する必要がある。

### 双方向ストリーミング
RESTにない機能として、
gRPCは1つの接続で
複数のリクエストとレスポンスを同時に送受信できる（双方向ストリーミング通信）。

### gRPCが適している場面
- 高性能なシステム
- 送信するデータ量が多い
- リアルタイム、またはストリーミングアプリケーション

## 実際に試す
[[gRPC_動かしてみる]]

## 参考
[gRPC](https://grpc.io/)
[Protocol Buffers Documentation (protobuf.dev)](https://protobuf.dev/)
[gRPC と REST の比較 - アプリケーション設計の違い - AWS (amazon.com)](https://aws.amazon.com/jp/compare/the-difference-between-grpc-and-rest/)
[【NTT西日本】RPC（Remote Procedure Call）｜ICT用語集｜法人・ 企業向け ICT サービス ・ ソリューション (ntt-west.co.jp)](https://www.ntt-west.co.jp/business/glossary/words-00229.html)
