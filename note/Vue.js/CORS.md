CrossOriginResourcePolicy

自分のサーバーから楽天のサーバーなど、異なる[[オリジン]]間で通信しようとしてもアクセスできないように基本的になっている。

サーバー側からのレスポンスヘッダーにて、Access-Control-Allow-Originで許可するオリジンを指定してやることで、特定のオリジンからのリクエストを受け付ける。

その他の関連：[[Preflight Request]]