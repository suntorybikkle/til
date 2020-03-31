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

## ハンドラーの追加

```go
func editHandler(w http.ResponseWriter, r *http.Request) {
    title := r.URL.Path[len("/edit/"):]
	p, err := loadPage(title)
	if err != nil {
		p = &Page{Title: title}
	}
	fmt.Fprintf(w, "<h1>Editing %s</h1>"+
		"<form action=\"/save/%s\" method=\"POST\">"+
		"<textarea name=\"body\">%s</textarea><br>"+
		"<input type=\"submit\" value=\"Save\">"+
		"</form>",
	p.Title, p.Title, p.Body)
}

func main() {
    http.HandleFunc("/view/", viewHandler)
	http.HandleFunc("/edit/", editHandler)
	http.HandleFunc("/save/", saveHandler)
	log.Fatal(http.ListenAndServe(":8080", nil))
}
```

- このようにして各ページごとにハンドラーを設定できる

## ハンドラーのラップ

```go
var templates = template.Must(template.ParseFiles("edit.html", "view.html"))

func renderTemplate(w http.ResponseWriter, tmpl string, p *Page) { # htmlのテンプレートを生成する
	err := templates.ExecuteTemplate(w, tmpl+".html", p)
	if err != nil {
		http.Error(w, err.Error(), http.StatusInternalServerError)
	}
}

func editHandler(w http.ResponseWriter, r *http.Request, title string) {
    p, err := loadPage(title)
	if err != nil {
		p = &Page{Title: title}
	}
	renderTemplate(w, "edit", p)
}

var validPath = regexp.MustCompile("^/(edit|save|view)/([a-zA-Z0-9]+)$") # URLの簡単なバリデーション

func makeHandler(fn func(http.ResponseWriter, *http.Request, string)) http.HandlerFunc {
    return func(w http.ResponseWriter, r *http.Request) {
		m := validPath.FindStringSubmatch(r.URL.Path)
		if m == nil {
			http.NotFound(w, r)
			return
		}
		fn(w, r, m[2])
	}
}

func main() {
    http.HandleFunc("/view/", makeHandler(viewHandler))
	http.HandleFunc("/edit/", makeHandler(editHandler))
	http.HandleFunc("/save/", makeHandler(saveHandler))
			
	log.Fatal(http.ListenAndServe(":8080", nil))
}															
```

- 既存の実装では、htmlをすべての関数で毎度生成していることと、バリデーション(今までなかったけど)操作はすべての関数で行うことが問題だった時の対処
- 上記の例では、`makeHandler`でラップすることにより解決している
