# Web Application

## シンプルな例

goのドキュメントにあったミニマムなウェブアプリケーション

```go:main.go
package main

import (
	"fmt"
	"log"
	"net/http"
)
			
func handler(w http.ResponseWriter, r *http.Request) {
	fmt.Fprintf(w, "Hi there, I love %s!", r.URL.Path[1:])
}
				
func main() {
	http.HandleFunc("/", handler)
	log.Fatal(http.ListenAndServe(":8080", nil))
}
```

- `http.ListenAndServe`をすると、指定したポートを開いてリクエストを受ける準備ができる
- `log.Fatal`はエラーを得たら、プログラムを`defer`などを無視して止めるというような意味。今回は気にしない
- `http.HandleFunc`は開けたポートに対してのリクエストごとにハンドルしてさばく処理を設定している (今回はルートが来たら`Hi`からの分を表示している)
- `http.ListenAndServe`にはハンドルされなかった時の処理が第二引数んひある。
  - `nil`はデフォルトの404表示. と思うけど、ルートで宣言しているからか、すべてハンドルされる
