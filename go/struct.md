# Struct

## メソッド

```go
type Human struct {
	name string
}

func (h Human) Say() {
	fmt.Println(h.name)
}

func (h *Human) Rename() {
	h.name = "Ken"
}

func main() {
	h := Human{"Tom"}
	h.Say()
	h.Rename()
	h.Say()
}
```

- 上記のように定義することで、`Human Struct`にメソッドを追加していることになる
- また、structの中身自体を変更したい場合はポインタレシーバ`Rename`のようにする
