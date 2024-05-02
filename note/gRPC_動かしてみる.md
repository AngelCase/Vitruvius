今回はGoでサンプルを動かす。
[Quick start | Go | gRPC](https://grpc.io/docs/languages/go/quickstart/)

## 事前準備
- Go：[Download and install - The Go Programming Language](https://go.dev/doc/install)
- プロトコルバッファコンパイラ`protoc`のバージョン3：[Protocol Buffer Compiler Installation | gRPC](https://grpc.io/docs/protoc-installation/)
- プロトコルコンパイラのためのGoプラグイン
  - プラグインをインストールする
    ```sh
	go install google.golang.org/protobuf/cmd/protoc-gen-go@v1.28
	go install google.golang.org/grpc/cmd/protoc-gen-go-grpc@v1.2
	```
  - PATHを通す
    ```sh
	export PATH="$PATH:$(go env GOPATH)/bin"
    ```

## サンプルコードの準備
サンプルコードのリポジトリをクローンし、
ディレクトリに移動。
```sh
git clone -b v1.60.1 --depth 1 https://github.com/grpc/grpc-go
cd grpc-go/examples/helloworld
```

## サンプルの実行
`examples/helloworld`で以下のコマンドを実行していく。
サーバーのコードをコンパイルし実行。
```sh
go run greeter_server/main.go
```
別のターミナルからクライアントのコードをコンパイルして実行。
```sh
go run greeter_client/main.go
```
以下の画面出力が出たら成功。
```
Greeting: Hello world
```

## gRPCサービスのアップデート
`helloworld/helloworld.proto`に、以下のように`SayHelloAgain()`メソッドを追加する。
```protobuf
// The greeting service definition.
service Greeter {
  // Sends a greeting
  rpc SayHello (HelloRequest) returns (HelloReply) {}
  // Sends another greeting
  rpc SayHelloAgain (HelloRequest) returns (HelloReply) {}
}
```

このメソッドを使うには、リコンパイルが必要となる。
`examples/helloworld`から以下を実行：
```sh
protoc --go_out=. --go_opt=paths=source_relative \
    --go-grpc_out=. --go-grpc_opt=paths=source_relative \
    helloworld/helloworld.proto
```

これでサーバーとクライアントのコードは再生成されたが、
人手で書かれた部分に新しいメソッドを追加する必要がある。

サーバー側のコード`greeter_server/main.go`に次の関数を追加。
```go
func (s *server) SayHelloAgain(ctx context.Context, in *pb.HelloRequest) (*pb.HelloReply, error) {
        return &pb.HelloReply{Message: "Hello again " + in.GetName()}, nil
}
```
クライアント側のコード`greeter_client/main.go`の`main()`の最後にに以下のコードを追加。
```go
r, err = c.SayHelloAgain(ctx, &pb.HelloRequest{Name: *name})
if err != nil {
        log.Fatalf("could not greet: %v", err)
}
log.Printf("Greeting: %s", r.GetMessage())
```

## 実行
`examples/helloworld`で以下のコマンドを実行していく。
サーバーのコードをコンパイルし実行。
```sh
go run greeter_server/main.go
```
別のターミナルからクライアントのコードをコンパイルして実行。
```sh
go run greeter_client/main.go --name=Alice
```
以下の画面出力が出たら成功。
```
Greeting: Hello Alice
Greeting: Hello again Alice
```
