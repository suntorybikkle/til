# テンプレートエンジン

テンプレートにデータを組み込んで最終的なHTMLを作成する

## テンプレートエンジンの起動

```go
t, _ := template.ParseFiles("hoge.html")
t.Execute(w, "Hello Hoge")
```

- `template.ParseFiles`関数でテンプレートを解析
- `Execute`メソッドでテンプレートにデータを当てはめる

## アクション

### 条件アクション

```
{{ if arg }}
  コンテンツ
{{ else }}
  他のコンテンツ
{{ end }}
```

### イテレータアクション

```
{{ range array }}
  {{ . }}
{{ end }}
```

### 代入アクション

```
{{ with arg }}
  
{{ end }}
```
