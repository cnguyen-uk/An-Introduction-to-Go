# An Introduction to Go
*A compact introduction to using Go.*

Go, or Golang, is a programming language designed by Google employees. It is a simple high performance language designed to make code extremely easy to read and develop; has modern features like garbage collection and built-in concurrency; but misses some of the functionality that other languages have (such as OOP).

There are plenty of good, comprehensive guides to using Go available online, such as [The Official Go Language Specification](https://golang.org/ref/spec). The aim of this guide is the opposite - to present Go in a compact way for someone who is already familiar with at least one programming language.

## Table of Contents

- [Coding Standards](#coding-standards)
  * [Indentation](#indentation)
  * [Comments](#comments)
    + [Inline Comments](#inline-comments)
    + [Block Comments](#block-comments)
  * [Quotes](#quotes)
  * [Names](#names)
- [Introduction](#introduction)
  * [Compiling](#compiling)
  * [Basic Go Structure](#basic-go-structure)
  * [Importing Multiple Packages](#importing-multiple-packages)
- [Variables and Types](#variables-and-types)
  * [Types](#types)
  * [Variables](#variables)
  * [Constants](#constants)
- [The `fmt` Package](#the-fmt-package)
  * [Printing](#printing)
  * [Sprinting](#sprinting)
  * [Getting User Input](#getting-user-input)
- [Conditionals](#conditionals)
  * [The `if`, `else if` and `else` Statements](#the-if-else-if-and-else-statements)
  * [Scoped Short Declaration Statement](#scoped-short-declaration-statement)
  * [The `switch` Statement](#the-switch-statement)
- [Loops](#loops)
- [Randomisation and Seeding](#randomisation-and-seeding)
- [Functions](#functions)
- [Pointers and Addresses](#pointers-and-addresses)
  * [Addresses](#addresses)
  * [Pointers](#pointers)
  * [Dereferencing](#dereferencing)

## Coding Standards

All written code should follow a style guide to ensure that standards are kept consistent across any codebase and make code easier to read. Badly written code is difficult to scale, optimise, and debug. Such is the importance of high coding standards that this guide will discuss it as a separate section before any code is seen.

We follow the standards in [Effective Go](https://golang.org/doc/effective_go.html). In particular, this section will act as a reference for language-specific best practices for indentation, comments, quotes, and names, since these can vary across different programming languages.

### Indentation

The standard is to use tabs for indented code.

### Comments

Comments begin with `//`. Block comments have the option to be wrapped with `/* */`.

#### Inline Comments

Inline comments should be used sparingly and not state the obvious. They should be separated by at least two spaces from the code and use commanding language rather than descriptive. For example, the following is preferred:

```Go
x = x + 1  // Compensate for border
```

As a comparison, the following should be avoided:

```Go
x = x + 1  // Compensates for border
```

#### Block Comments

Block comments should be written in complete sentences, be indented with the code which it is commenting, and come *before* the code which it is commenting. Placing the comment after is merciless on a confused reader. Paragraphs are separated by a single line containing a single `//` (unless the block comment is already wrapped in `/* */`), and multi-sentence comments should have two spaces after a sentence-ending period (except after the final sentence).

The norm is to use `//` for block comments. Usage of `/* */` is typically seen for package comments.

```Go
// This function triples a number.  It does not check for valid input.
//
// It's quite a nice function.
func triple(x int) (int) {
	return 3*x
}
```

### Quotes

Strings can be wrapped in either double quotes `" "` or backticks `` ` ` ``. Double quotes will honour escaping characters, whereas backticks will not, i.e. they contain raw literal strings.

Single quotes `' '` are used to declare [runes](https://golang.org/doc/go1#rune).

### Names

Package names should be short, single-word, and lowercase.

In general, names which start with an uppercase character will be visible outside of the package in which they were defined. The convention for multiword names is *PascalCase* or *camelCase*.

## Introduction

### Compiling

Compiling in Go is done using the command line.

```Bash
go build file.go
./file
```

```Bash
go run file.go
```

The first way compiles `file.go`, which is stored in an executable file, and then runs it. The second way simply runs `file.go` without creating an executable file.

### Basic Go Structure

Go programs are read as normal - from top to bottom, left to right.

```Go
package main

import "fmt"

func main() {
	fmt.Println("Hello World")
}
```

Almost all Go programs will use the `main` package since it will create an executable file. It is a special package containing the `main()` function which will execute any code inside of its scope.

The `fmt` package allows for some C type functionality in formatting I/Os. Functions are defined using the `func` keyword. Both of these will be discussed later on.

### Importing Multiple Packages

To import multiple packages we can use multiple lines with import statements. Or we can use a single import statement. We may also include alias names.

```Go
import(
	"package1"
	p2 "package2"
)
```

## Variables and Types

Go is statically-typed, so variables need to be given a type before usage.

### Types

See the [documentation on types](https://golang.org/ref/spec#Types). Commonly used types for typical programming are the Boolean, numeric, and string types.

### Variables

Variable declaration may be done in a few ways.

```Go
var variableName type
variableName = value
```

```Go
variableName := value
```

```Go
var variableName = value
```

The first way explicitly declares the variable and its type before initialising it by assigning a value to it. The second way uses the short declaration operator `:=`, which allows Go to infer its type based on the value provided. The third way is alternate syntax for the second way.

We may also define multiple variables of the same type in a single line.

```Go
var variableName1, variableName2 type
variableName1 = value1
variableName2 = value2
```

```Go
variableName1, variableName2 := value1, value2
```

```Go
var variableName1, variableName2 = value1, value2
```

If a variable is declared but not initialised, then Go will give it a default zero value (depending on type). Not using a variable will throw an error (to catch unused code).

### Constants

Constants can't be changed later in the program, which allows the developer to highlight their intention. These are declared similarly to variables, but can only be character, string, Boolean or numeric types.

```Go
const gravity = 9.80665
```

## The `fmt` Package

The [`fmt` package](https://golang.org/pkg/fmt/) provides familiar data printing and formatting capabilities.

### Printing

Here are some commonly used print functions:

```Go
fmt.Println("Hello everyone", "in the world")
fmt.Println("Goodbye")
/*
Print: Hello everyone in the world
       Goodbye
*/
```

```Go
fmt.Print("I would like", " to say: ")
fmt.Print("Hello")
// Print: I would like to say: Hello
```

```Go
/*
This will print as expected, but with the %v verbs replaced by verb1 and
verb2, in that order.  Other verbs apart from %v exist and can be seen in the
documentation.
*/
fmt.Printf("The best food is %v from %v", verb1, verb2)
```

### Sprinting

We can also choose to return instead of printing.

```Go
words := fmt.Sprintln("Hello everyone", "in the world")
```

```Go
words := fmt.Sprint("Hello everyone ", "in the world")
```

```Go
words := fmt.Sprintf("The best food is %v from %v", verb1, verb2)
```

### Getting User Input

The following syntax structure allows for us to ask for user input:

```Go
var response string
fmt.Scan(&response)
```

This stores the user's input as the variable `response`, which we can do anything with. Note that the `Scan()` function expects addresses as arguments, hence the `&`.

## Conditionals

Go has the usual comparison and logical operators: `==`, `!=`, `<`, `>`, `<=`, `>=`, `&&`, `||`, `!`.

### The `if`, `else if` and `else` Statements

As usual, a block of code is executed given that a condition is true.

```Go
x := someNumber
if x > 100 {
	fmt.Println("We have a large number")
} else if x > 20 {
	fmt.Println("We have a medium number")
} else {
	fmt.Println("We have a small number")
}
```

### Scoped Short Declaration Statement

It is possible to include a short variable declaration with our statements, but this variable is only accessible in the scope of the statement blocks.

```Go
if x := someNumber; x > 100 {
	fmt.Println("We have a large number")
}
```

### The `switch` Statement

We can use a `switch` statement to handle cases more elegantly.

```Go
x := someNumber
switch x {
case 5:
	fmt.Println("This is the maximum achievable!")
case 4:
	fmt.Println("This is almost the maximum achievable!")
default:
	fmt.Println("This is quite average.")
}
```

The `default` keyword deals with the rest of the cases.

## Loops

Go has only one looping construct - the `for` loop. The basic syntax is as follows:

```Go
for i := 1; i < 8; i++ {  // Shorthand for i += 1
	statement
}
```

## Randomisation and Seeding

One way to generate random numbers in Go is as follows:

```Go
package main

import (
	"fmt"
	"math/rand"
	"time"
)

func main() {
	rand.Seed(time.Now().UnixNano())
	fmt.Println(rand.Intn(100))
}
```

Without the `rand.Seed(time.Now().UnixNano())` line of code, the following line would print the same random number every time the code is run. This is because Go chooses a random number using a seed as a starting point, which by default is always `1`. To get around this, we can use `rand.Seed()` to generate a new seed. We can pass the time as an argument since it is guaranteed to be different every time the code is run. See the [documentation](https://golang.org/pkg/time/#Time.UnixNano) on `UnixNano` for more.

## Functions

The syntax for functions is quite standard, which we define outside of the scope of the `main()` function. If we wish to call functions, then we simply call them within the `main()` function as usual.

```Go
func specificCalculations(x int, y int) (int, int, float32) {
	defer fmt.Println("Calculations complete.")
	a := x + y
	b := x * y
	c := x/y
	return a, b, c
}
```

The first tuple defines the parameters, and their types, that the function takes, and the second tuple defines the types of the return.
The `defer` keyword can be used to delay the running of a function to the end of the current function, regardless of what happens in the scope of the current function. This can be useful in saving on repeated code in a function that may have multiple `return` statements.

## Pointers and Addresses

Go is a pass-by-value language, which means that it passes to functions the value of an argument. However, it is possible to pass the argument itself if we so wish.

### Addresses

When a variable is declared, it is stored in computer memory. The space in the computer in which this is allocated is called an address. Every time that variable is used, the computer retrieves the value stored at the variable's address. 
To find the address of a variable, we can prefix it with the `&` operator.

```Go
x := 152
fmt.Println(&x)  // Print: address of x in hexadecimal
```

### Pointers

A variable which stores an address is called a *pointer*. These allow us to access the address of a variable. A pointer can be declared and initialised using the `*` operator.

```Go
var variableName *type
variableName = &value
```

```Go
variableName := &value
```

```Go
var variableName = &value
```

### Dereferencing

We can change the value stored in an address to a different value. We can prefix the `*` operator to a pointer to assign a new value to that address. This is called *dereferencing* or *indirecting*.

```Go
x := 152
pointerForNumber := &x
*pointerForNumber = 7
fmt.Println(x)  // Print: 7
```

Through dereferencing, we can change the value of a variable in a different scope.

```Go
func addTen(pointerForNumber *int) {
	*pointerForNumber += 10
}

func main() {
	x := 53
	addTen(&x)
	fmt.Println(x)  // Print: 63
}
```
