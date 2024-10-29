# wtfgolang

What the f\*ck Golang? 🤨

### ❓`len("string")` is Not Really the Length of a String

```go
package main

import "fmt"

func main() {
  fmt.Println(len("Fіve"))
}
```

**Output**

```go
5
```

Well, it's obviously 5. Isn't it? 😅  
Don't trust me? Run it yourself in [The Go Playground](https://go.dev/play/p/BUU_QMh6xLl) 😜

#### 💡 Explanation

Ok it's 2 AM and I'm going to bed now. I'll update this tomorrow. 😴

---

### ❓When `nil != nil`

```go
package main

import "fmt"

type MyError struct{}

func (c MyError) Error() string {
  return "I ❤️ Golang"
}

func returnMyError() error {
  var err *MyError = nil
  return err
}

func main() {
  err := returnMyError()
  fmt.Println(err) // this prints <nil>
  if err == nil {
    fmt.Println("Of course it's nil, right?")
  } else {
    fmt.Println("Well, actually it's not 😄")
  }
}
```

**Output**

```go
<nil>
Well, actually it's not 😄
```

Example available in [The Go Playground](https://go.dev/play/p/sg78qnOtqpe)

#### 💡 Explanation

Ok it's 2 AM and I'm going to bed now. I'll update this tomorrow. 😴

---
