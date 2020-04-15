# cookie

ユーザーがログイン済みかどうか判断するために、クッキーを使用する

- クッキーはレスポンスヘッダでユーザに渡され、ブラウザに保存する
- ユーザは今後、リクエストヘッダにクッキー情報を含める

## クッキーを発行する

```go
func authenticate(w http.ResponseWriter, r *http.Request) {
	r.ParseForm()
	user, _ := data.UserByEmail(r.PostFormValue("email"))
	if user.Password == data.Encrypt(r.PostFormValue("password")) {
		session := user.CreateSession()
		cookie := http.Cookie{
			Name: "_cookie",
			Value: session.Uuid,
			HttpOnly: true,
		}
		http.SetCookie(w, &cookie)
		http.Redirect(w, r, "/", 302)
	} else {
		http.Redirect(w, r, "/login", 302)
	}
}
```

- ユーザのログイン時の情報をパースして、存在するユーザかどうかの判断を行う
  - `data.Encrypt`は文字列の暗号化を行っている行っている
- ユーザ認証が済めば、セッションとクッキーを作成する
- レスポンスヘッダにクッキーを含めて返す

## ログインしているかどうかの判断

```go
func session(w http.ResponseWrite, r *http.Request) (sess data.Session, err error) {
	cookie, err := r.Cookie("_cookie")
	if err == nil {
		sess = data.Session{Uuid: cookie.Value}
		if ok, _ := sess.Check(); !ok {
			err = errors.New("Invalid session")
		}
	}
	return
}
```

- クッキーが存在するかの確認
- セッションのユニークidが孫座するかの確認 `sess.Check()`
- 上記のようなクッキー確認を各ハンドラに用いて処理を分けたりする
  - ログイン済みのユーザにはログイン情報を含めたページの表示をするなど
