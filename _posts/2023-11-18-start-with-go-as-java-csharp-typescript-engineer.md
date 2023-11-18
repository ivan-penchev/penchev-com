---
title: "Transition to Go as a Java, C#, Typescript or other engineer"
author: ivan
date: 2023-11-18 12:22:21 +0100
categories: [Programming]
tags: [Go]
toc: true
mermaid: true
---

## Initial struggles with Go

Junior, Mid, Senior, or even more expirienced engineer - it does not matter. If you have some expirience with some OOP language, embarking on journey with Go feels like navigating through uncharted waters. The shift from the Object-Oriented Programming (OOP) paradigm, where everything fits neatly into classes and patterns, has been both liberating and challenging. Go introduces fundamental differences—no more inheritance, a breath of fresh air, and the elevation of functions to first-class citizens.

But worry not, the journey would be a reward one. Go is awesome!

## Learning resources

I think a big challenge, that most starting with Go have, is finding relative resources.
I will try to split this by level of expirience "needed", however with a enough effort one can finish any read regarless of their expirience.

* All

1. [Roadmap: Go Developer](https://roadmap.sh/golang)
2. [Go Class by Matt KØDVB](https://www.youtube.com/playlist?list=PLoILbKo9rG3skRCj37Kn5Zj803hhiuRK6)
3. [How to "Go project folder structure"](https://github.com/golang-standards/project-layout)

* 1-5 years of expirience

1. [Learn Go Programming - Golang Tutorial for Beginners by Freecodecamp](https://www.youtube.com/watch?v=un6ZyFkqFKo)
2. [Go 101](https://go101.org/article/101.html)
3. [Golang Interfaces Explained](https://www.alexedwards.net/blog/interfaces-explained)
4. [Learn Go with Tests](https://quii.gitbook.io/learn-go-with-tests/)

* 5+ years of expirience
  
1. [100 Go Mistakes and How to Avoid Them](https://100go.co/)
2. [Visualize Go slices and arrays](https://www.willem.dev/projects/slice-visualizer)
3. [A Guide to the Go Garbage Collector](https://tip.golang.org/doc/gc-guide)

* Transitioning from other languages

1. [Go for Java Developers](https://java2go.dev/)
2. [Introduction to Go for senior (Python) developers](https://www.augmentedmind.de/2023/01/22/go-vs-python-senior-developers/)

## Some Go peculiarities 

### Forget about immutability. 
Go does not have built-in support for immutability. Variables are mutable by default.

```go
package main

import "fmt"

func main() {
    var x int
    x = 5
    fmt.Println(x) // Output: 5

    x = 10
    fmt.Println(x) // Output: 10
}

```

### Forget about getters/setters overall. 
Go encourages direct access to struct fields.

```go
package main

import "fmt"

type Person struct {
    FirstName string
    LastName  string
}

func main() {
    p := Person{FirstName: "John", LastName: "Doe"}
    fmt.Println(p.FirstName) // Direct access, no need for getters
}
```

### Do not use panic for errors - return errors instead. 
Use error wrapping and errors.Is/As for error type checking.

```go
package main

import "fmt"

func modifyValue(x int) {
    x = 10
}

func modifyPointer(x *int) {
    *x = 10
}

func main() {
    value := 5
    modifyValue(value)
    fmt.Println(value) // Output: 5

    modifyPointer(&value)
    fmt.Println(value) // Output: 10
}

```

###  Nil slices/maps are ok, even best practice.
You don't need to return empty ones for example.

```go
package main

import "fmt"

func main() {
    var slice []int
    var m map[string]int

    fmt.Println(slice == nil) // Output: true
    fmt.Println(m == nil)     // Output: true
}

```

###  Zero values are automatically assigned to variables when they are declared. 
Embrace zero values and make use of them. Forget about "optional" and nil-avoidance.

```go
package main

import "fmt"

type Point struct {
    X int
    Y int
}

func main() {
    var p Point
    fmt.Println(p.X, p.Y) // Output: 0 0
}

```

###   Early returns everywhere. 
Use early returns to reduce indentation.

```go
package main

import "fmt"

func validateInput(x int) error {
    if x < 0 {
        return fmt.Errorf("input cannot be negative")
    }
    return nil
}

func main() {
    err := validateInput(-5)
    if err != nil {
        fmt.Println("Error:", err)
        return
    }

    fmt.Println("Validation successful")
}

```

###  Concurrency - learn channels and select statement well.
When using a sync.Mutex or concurrent data structures, ask yourself if you can solve the problem using channels instead.

```go
package main

import "fmt"

func validateInput(x int) error {
    if x < 0 {
        return fmt.Errorf("input cannot be negative")
    }
    return nil
}

func main() {
    err := validateInput(-5)
    if err != nil {
        fmt.Println("Error:", err)
        return
    }

    // Rest of the code if validation is successful
    fmt.Println("Validation successful")
}

```

### Use context package for timeouts/cancellations. 

```go
package main

import (
    "context"
    "fmt"
    "time"
)

func performTask(ctx context.Context) error {
    select {
    case <-ctx.Done():
        return ctx.Err()
    default:
        // Do some work
        time.Sleep(2 * time.Second)
        return nil
    }
}

func main() {
    ctx, cancel := context.WithTimeout(context.Background(), time.Second)
    defer cancel()

    if err := performTask(ctx); err != nil {
        fmt.Println("Error:", err)
    }
}

```

### Accept interfaces, return structs (Not always possible, but learn it)

```go
package main

import "fmt"

type Shape interface {
    Area() float64
}

type Circle struct {
    Radius float64
}

func (c Circle) Area() float64 {
    return 3.14 * c.Radius * c.Radius
}

func calculateArea(s Shape) float64 {
    return s.Area()
}

func main() {
    circle := Circle{Radius: 5}
    result := calculateArea(circle)
    fmt.Println("Area:", result)
}

```

### Mind the package names when naming structs/interfaces.
For the package `backup`, the type `Store` could be good name for a type, not `BackupStore`. From other packages, it would read as `backup.Store`.

### Use `defer` 
but don't use `defer` INSIDE a for loop.

```go
package main

import "fmt"

func main() {
    fmt.Println("Start")
    defer fmt.Println("Deferred statement")
    fmt.Println("End")
}
```



