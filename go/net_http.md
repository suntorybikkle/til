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

## マルチプレクサ

- マルチプレクサでは要求されたURLに対応したハンドラを呼ぶ
- これは、`DefaultServeMux`のハンドラがそのように実装されているためである
- つまり、別のハンドラ (サードパーティー製のマルチプレクサ) を`http.Server`に登録することもできる
- URLは最も近いURLとマッチングするようになっているが、`/hello`では完全一致しか呼ばれないため、`/hello/`のようにする必要がある

## Request

- URL
  - `scheme://[userinfo@]host/path[?query][#fragment]`の構成要素がURL構造体のフィールドとなる
- リクエストヘッダ
  - キーが文字列型、値が文字列型のスライスのマップ
- ボディ
  - メソッド`Read`と`Close`を持っている
- フォーム
  - クライアントのHTMLフォームを利用したPOSTリクエストを受け取る
  - ParseFormなどを利用してリクエストを解析する

## クッキー

Goでは以下のような構造体でCookieを表現する

```go
type Cookie struct {
     Name       string
     Value      string
     Path       string
     Domain     string
     Expires    time.Time
     RawExpires string
     MaxAge     int
     Secure     bool
     HttpOnly   bool
     Raw        string
     Unparsed   []string
}
```

- クッキーの種類は基本的にはセッションクッキーと永続性クッキーの２種類
- クッキーの有効期限は`Expires`か`MaxAge`で指定される
- 有効期限が指定されていないクッキーがセッションクッキーとなる

クッキーの設定

```go
c := http.Cookie{
  Name:  "hoge_cookie"
  Value: "Hello world!"
}
http.SetCookie(w, &c)
```

- クッキーを参照渡ししている点に注意
- 別の設定方法に`w.Heder().Set("Set-Cookie", c.String())`という書き方もある

クッキーの読み取り

```go
c, err := r.Cookie("hoge_cookie")
if err != nil {
   fmt.Fprintln(w, "Cannot get hoge_cookie")
}
```

- `Cookie`メソッドはクッキーが取得できなかった時、エラーを返す
- 複数のクッキーを取得する場合、`Cookies`メソッドを使う
- ヘッダーを直接読む場合、`r.Header["Cookie"]`と書くことで取得できる
