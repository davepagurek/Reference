# Go

## Setup, variables
```go
package main
import "fmt"

func main() {
  var a string = "test"
  b := "test"
  var intptr *int = new(int)
  var slice1 []string = make([]string, 3)
  slice2 := []string{"a", "b", "c"}
  copy(slice1, slice2) // copy(dest, source)
}
```

## I/O 
```go
import("os"; "fmt"; "bufio"; "strings")

// Slurp file
dat, err := ioutil.ReadAll("filename")

// Read file
f, err := os.Open("filename")
scanner := bufio.NewScanner(f)
// f, os.Stdin, strings.NewReader(str), 
scanner.Split(bufio.ScanLines)
// ScanBytes, ScanLines, ScanRunes, ScanWords
for scanner.Scan() {
  fmt.Printf("%v\n", scanner.Text())
}
```

## Data structures
```go
import ("fmt"; "encoding/json"; "math")

// Maps
hash1 := make(map[string]int)
hash1["something"] = 3
hash2 := map[string]int{
  "test": 42,
  "test2": 3
}
for key, val := range hash2 {
  fmt.Println(key, val)
}
marshalled, err := json.Marshal(hash2)
fmt.Printf("%s\n", string(marshalled))

// Arrays/slices
arr := [...]int{1, 2, 3}
slice1 := arr[0:len(arr)]
slice2 := arr[:]
slice3 := make([]int, 0) // make([]int, len, max)
slice3 = append(slice3, 2)
slice3 = append(slice3, slice2)
slice3 = append([]int{42}, slice3) // prepend
for i, val := range slice3 {
  fmt.Println(i, val)
}

// Structs, interfaces
type Square struct { // Capitals are exported
  length, width float
}
type Circle {
  r float `json:"radius"`
}
type Shape interface {
  area() float
}
func (s Square) area() float {
  return s.length * s.width
}
func (c Circle) area() float {
  return math.Pi * math.Pow(c.r, 2)
}
for shape := range []Shape{Square{2,2}, Circle{3}} {
  fmt.Println(shape.area())
}
```

## Concurrency
```go
func gen(nums ...int) <-chan int {
  out := make(chan int)
  go func() {
    for _, n := range nums {
      out <- n
    }
    close(out)
  }()
  return out
}

func sq(in <-chan int) <-chan int {
  out := make(chan int)
  go func() {
  for n := range in {
    out <- n * n
  }
  close(out)
  }()
  return out
}

func merge(cs ...<-chan int) <-chan int {
  var wg sync.WaitGroup
  out := make(chan int)

  // Start an output goroutine for each input channel in cs.  output
  // copies values from c to out until c is closed, then calls wg.Done.
  output := func(c <-chan int) {
    for n := range c {
      out <- n
    }
    wg.Done()
  }
  wg.Add(len(cs))
  for _, c := range cs {
    go output(c)
  }

  // Start a goroutine to close out once all the output goroutines are
  // done.  This must start after the wg.Add call.
  go func() {
    wg.Wait()
    close(out)
  }()
  return out
}

func main() {
  in := gen(2, 3)

  // Distribute the sq work across two goroutines that both read from in.
  c1 := sq(in)
  c2 := sq(in)

  // Consume the merged output from c1 and c2.
  for n := range merge(c1, c2) {
    fmt.Println(n) // 4 then 9, or 9 then 4
  }
}
```
