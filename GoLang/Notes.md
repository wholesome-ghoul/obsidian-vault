## misc

```go
import "reflect"
import "runtime"

filepath.Clean(path)
// does not follow sym links
// less efficient than WalkDir
filepath.Walk(path, fn)
filepath.Base(path)
filepath.Dir(path)
filepath.Ext(path)

os.Lstat(path)
os.Stat(path)

// is just string(b1) == string(b2)
bytes.Equal(b1, b2)

// create a temporary file (max retries 10,000)
os.CreateTemp(dir, pattern)
// removes a file or an empty dir
os.Remove(name)
os.Chmod(name, mode)
os.Rename(name, newName)
os.MkdirAll(path, perm)

// get file (*os.File) name
f.Name()
f.Write(bytes)
f.Read(bytes)
f.Close()

runtime.NumCPU()

func (t T) String() string
```

## go cli
 ```bash
# view coverage as html
go tool cover -html="coverage.out"
go tool cover -html="coverage.out" -o coverage.html
# check cpu prof
go tool pprof cpu.pprof

# check if what flags are enabled
go version -m <binary>

# generate benchmark
go test -bench=. -run=xxx -benchmem -memprofile mem.pprof -cpuprofile cpu.pprof -benchtime=10s > 0.bench
# generate coverage
go test -coverprofile coverage.out
go test -cover
go test -race

# compare benchmarks
# go install golang.org/x/perf/cmd/benchstat@latest
benchstat 0.bench 1.bench

# go install golang.org/x/tools/cmd/benchcmp
benchcmp 0.bench 1.bench

# get doc
go doc sync.WaitGroup
godoc -http=:6060

# in the root
go work init
go work use <name-of-module>

go install github.com/kisielk/errcheck/errcheck

# profile guided optimization (pgo)
curl -o cpu.pprof "http://localhost:8080/debug/pprof/profile?seconds=30"
mv cpu.prof default.pgo
```

## goroutines

- by default, `sends` and `receives` block until the other side is ready.
- channels can be buffered.
- only `sender` should close a channel, never `receiver`. Sending on a closed channel will cause a panic.
- closing is not always necessary.

```go
// `select` example
func fibonacci(c int, quit chan int) {
	x, y := 0, 1
	for {
		select {
			case c <- x:
				x, y = y, x + y
			case <- quit:
				fmt.Println("quit")
				return
		}
	}
}

func main() {
	c := make(chan int)
	quit := make(chan int)
	go func() {
		for i := 0; i < 10; i++ {
			fmt.Println(<-c)
		}
		quit <- 0
	}()
	fibonacci(c, quit)
}
```

## mutex

```go
type SaveCounter struct {
	mu sync.Mutex
	v map[string]int
}

func (c *SafeCounter) Inc(key string) {
	c.mu.Lock()
	c.v[key]++
	c.mu.Unlock()
}

func (c *SafeCounter) Value(key string) int {
	c.mu.Lock()
	defer c.mu.Unlock()
	return c.v[key]
}

func main() {
	c := SafeCounter{v: make(map[string]int)}
	var wg sync.WaitGroup
	for i := 0; i < 1000; i++ {
		wg.Add(1)
		func() {
			defer wg.Done()
			go c.Inc("somekey")
		}()
	}

	wg.Wait()
	fmt.Println(c.Value("somekey"))
}
```

## tests

```go
import "testing"

func TestXxx(t *testing.T) {
	tableTests := []struct {
		name string
		expected int
	} {
		{name: "test name 1", expected: 1},
		{name: "test name 2", expected: 2},
	}

	for _, tt := range tableTests {
		t.Run(t.name, func(t *testing.T){
			// ...
		})
	}
}

func ExampleXxx() {
	got := Xxx()
	fmt.Println(got)
	// Output: 6
}
```

## parse go file comments

```go
package main

import (
	"fmt"
	"go/token"
	"go/parser"
)

func main() {
	// ou yeah baby
	fileSet := token.NewFileSet()
	ast, _ := parser.ParseFile(fileSet, "main.go", nil, parser.ParseComments)

	for _, comment := range ast.Comments {
		fmt.Println(comment.Text())
	}
}
```
