# wtfgolang

What the f\*ck Golang? 🤨

### 🔴 `len("string")` is not really length of a string

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
