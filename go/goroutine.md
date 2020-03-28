# goroutine

非同期処理を簡単に行ってくれるもの

## 使い方


```go
import (
	"fmt"
	"sync"
)

func goroutine(s []string, wg *sync.WaitGroup) {
	defer wg.Done()
	for _, v := range s{
		fmt.Println(v)
	}
}
							
func main() {
	var wg sync.WaitGroup
	test := []string{"s1", "s2", "s3", "s4"}

	for i := 0; i < 4; i++ {
		wg.Add(1)
		go goroutine(test, &wg)
	}
	wg.Wait()
}
```

- `go`でその関数がgoroutineとなり非同期で処理が行われる
- 今回の場合、非同期処理のため`wg.Wait()`がなければ、`main()`関数をすぐに抜けて何も表示されない
- `sync.WaitGroup`は`Add()`している個数だけの非同期処理が行われているとき、その数だけ`Done()`が呼ばれることを待っている
