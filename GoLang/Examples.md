---
tags: go examples
deck: go
---

# select example in golang #card
<!-- 1700392222352 9e1d2dbd97fb795d592040d7341e47a3 -->

```go
func fib(c, quit chan int) {
  x, y := 0, 1
  for {
    select {
      case: c<-x:
        x, y = y, x+y
      case: <-quit:
        fmt.Println("QUIT")
        return
    }
  }
}

func main() {
  c := make(chan int)
  quit := make(chan int)
  go func() {
    for i:=0; i<10; i++ {
      fmt.Println(<-c)
    }
    quit <- 0
  }()

  fib(c, quit)
}
```
