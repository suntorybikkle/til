# net/http ライブラリ

## Web サーバーの最小構成

```go
package main

import (
	    "net/http"
)

func main() {
	http.ListenAndServe("", nil)
}
```

## サーバー構造体

```go
type Server struct {
	Addr        string
	Handler     Handler
	ReadTimeout time.Duration

	// ...
	
}
```

上記、サーバー構造体を利用することで、サーバーの設定を変更する (下記はカスタマイズ例)

```go
func main() {
	server := http.Server{
		Addr:    "127.0.0.1:8080",
		Handler: nil,
	}
	server.ListenAndServe()
```

## ハンドラのチェイン

```go
func log(h http.HandlerFunc) http.HandlerFunc {
	return func(w http.ResponseWrite, r *http.Request) {
		name := runtime.FuncForPC(reflect.ValueOf(h).Pointer()).Name()
		fmt.Println("Hndler function called - " + name)
		h(w, r)
	}
}
```

- 引数HandlerFuncで返り血もHandlerFuncである
- `http.HandleFunc("/hoge", log(hoge))` のように利用することで、ログ出力を挟み込んでいる
- 上記のような使い方をすることで、ロジックの分離を行う

