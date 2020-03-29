# package

Goはすべてのパッケージを一つのGOPATHで管理することが推奨されている模様  
つまり以下のような構成で開発を進めることになる

```
GOPATH
├── bin
├── pkg
└── src
	├── github.com
	├── golang.org
	├── example1Project
	|	├── main.go
	|	├── package1
	|	|	└── hoge.go
	|	└── package2
	└── example2Project
```

## 自作パッケージの呼び出し方

```go:hoge.go
ackage package1

func Hoge() string {
    return "hoge"
}
```

```go:main.go
ackage main

import (
    "fmt"
	"example1Project/package1"
)
		
func main() {
	fmt.Println(package1.Hoge())
} // hoge
```

- package名でインポートするため、ファイル名は関係ない
- インポート時はGOPATHからのパスを書く
- より深い場所にある場合は`/`で続けて読み込む
