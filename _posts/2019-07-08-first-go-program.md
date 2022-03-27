---
title: First Go program
date: 2019-07-08 19:12:00 +0530
categories: [Technical, Tutorial]
tags: [go]
---

Every Go source file belongs to a package. A package is nothing but a collection of files. Here by defining the package as `main`, we are telling the Go compiler to compile the package as an executable file and not as a shared library. When you compile a file in main package, Go gives us a binary file which when executed calls the `main` function (the starting point of our program).

The `import` statement is used to import the standard libraries which we download when we install Go. The standard libraries were provided by the developers of Go to make our lives easier and contain commonly used functions. If you have used other programming languages like C or Java, this will not be alien to you.

In the `main` function, we call the `Println` function defined in the `fmt` package to print our output to the console. `fmt` has many functions to display output and each of them have their own use case.

That's it ! You have just written your first program in Go.

```go
package main

import (
	"fmt"
)

func main() {
	fmt.Println("My first Go program.")
}
```