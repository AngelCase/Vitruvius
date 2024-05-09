# Vitestのカバレッジレポートでファイル名などが...で省略されないようにする

## 課題

Vitestでテストを書いている。

カバレッジを`vitest run --coverage`で出力しているのだが、

ファイルのパスが長すぎて以下のように3点`...`で省略されてしまうことがあった。

  

```

---------------------------|---------|----------|---------|---------|-------------------

| File                      | % Stmts | % Branch | % Funcs | % Lines | Uncovered Line #s |

|---------------------------|---------|----------|---------|---------|-------------------|

| All files                 | 83.26   | 78.63    | 69.42   | 83.26   |                   |

| src                       | 0       | 0        | 0       | 0       |                   |

| ...eComponents/FugaButton | 0       | 0        | 0       | 0       |                   |

| index.vue                 | 0       | 0        | 0       | 0       |                   |

```

  

以下のように省略せずに出力したい。

```

| src/hogeComponents/FugaButton | 0       | 0        | 0       | 0       |                   |

```

  

現状、`vitest.config.ts`は以下のようになっていて、

`provider`に`v8`、`reporter`に`text`を指定している。

```ts

export default defineConfig({

  test: {

    （省略）

    coverage: {

      provider: "v8",

      reporter: ["text"]

    }

  }

});

```

  

## 解決法

ファイル名が...で潰れてしまう原因は、表の幅が狭いから。

`text`をreporterとして使う場合、`maxCols`オプションがあるため、

それを使って幅を広げてやれば良い。

  

以下のように設定できる。

```

{

  coverage: {

    provider: "v8",

    reporter: [

      ['text', { maxCols: 200 }]

    ]

  }

}

```

  

## 参考

[https://github.com/vitest-dev/vitest/discussions/5270](https://github.com/vitest-dev/vitest/discussions/5270)