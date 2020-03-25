# ステートメント

## defer

関数内で最後に遅延して実行される。\
ファイルをクローズする時とかに使う

```go
func foo() {
     defer fmt.Println("defer")
     fmt.Println("first")
} // first → deferの順で出力

func hoge() {
     file, _ := os.Open("path")
     defer file.Close()
     // 処理
}
```

defer はスタッキングされるため、deferで書かれた中で、一番最初のものが最後に実行される
