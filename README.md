# <img height=25 src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/scala/scala-original.svg" /> Functional Programming in Scala

This document aims to present a simple but useful introduction to the core concepts of functional programming with examples in Scala.
Although the examples are written in Scala, most of the concepts presented here are general to the functional programming paradigm and can be easily applied to other languages.

If you have any doubts or suggestions about the content, feel free to contribute opening an issue or a pull request.

## Topics

- [Introduction](#introduction)
- [Substitution Model](#substitution-model)
- [Pure Functions](#pure-functions)
    - [Impure Functions are Needed](#impure-functions-are-needed)
    - [Key Points](#key-points)
- [Immutability](#immutability)
    - [Key Points](#key-points-1)
- [Higher-order Functions](#higher-order-functions)
- [Closures](#closures)
- [Functional Composition](#functional-composition)
- [Currying](#currying)
- [Map, Filter and Reduce](#map-filter-and-reduce)
    - [Map](#map)
    - [Filter](#filter)
    - [Reduce](#reduce)
    - [Mixing the Trio](#mixing-the-trio)
- [Referential Transparency](#referential-transparency)
- [Tail Recursion](#tail-recursion)
- [Enumerations](#enumerations)
- [Anonymous Functions](#anonymous-functions)
- [Option, Some, None](#option-some-none)
- [Try, Success, Failure](#try-success-failure)
- [Companion Objects](#companion-objects)
- [Case Classes](#case-classes)
- [Short-circuit Evaluation](#short-circuit-evaluation)
- [References](#references)

## Introduction

Like many other modern programming languages, Scala is a multi-paradigm language which supports concurrent, functional, imperative and object-oriented paradigms. Despite that, in this document we're going to focus only in the functional paradigm.

> Note that all examples presented below can be reproduced in the Scala REPL

## Substitution Model

## Pure Functions

According to the [Wikipedia](https://en.wikipedia.org/wiki/Pure_function) and many other authors, pure functions are functions that have the following properties:

- The function **return value will always be the same**: no matter how many times you call the function, it should always return the same value or values for the same input parameters.
- The function **has no side effects**: the function should not interact with the external world (I/O, databases, external APIs, etc). For this assumption to make sense, the function should always have a return value.

Based on the assumptions above, a pure function is a computational analogue of a mathematical function.

Also, since pure functions are deterministic, they are often easier to test and even understand.

Here is a simple example of a **pure function** that sum `x` and `y`:

```scala
scala> def sum(x: Int, y: Int): Int = x + y
def sum(x: Int, y: Int): Int

scala> sum(10, 5)
val res0: Int = 15

scala> sum(10, 5)
val res1: Int = 15
```

As you can see, the return value (output) depends only on the function parameters (input values).

Now, let's see an example of an **impure function**:

```scala
scala> var y: Int = 5
var y: Int = 5

scala> def sum(x: Int): Int = x + y
def sum(x: Int): Int

scala> sum(10)
val res0: Int = 15

scala> y = 10
// mutated y

scala> sum(10)
val res1: Int = 20
```

As you can see, the `sum` function now depends on the `y` variable, which is in the outer scope. If we change the value of `y` it will affect the result of the `sum` function. So, even when we call the function with the same parameters it will return a different result.

Another example of an **impure function** can be seen in the following snippet:

```scala
scala> def sum(x: Int, y: Int): Int = x + y + scala.util.Random.nextInt(10)
def sum(x: Int, y: Int): Int

scala> sum(10, 5)
val res0: Int = 17

scala> sum(10, 5)
val res1: Int = 19
```

In the example above the `sum` function is using a random function to generate an integer value and adding it to the sum. So, even when we call the function with the same parameters it will return a different result.

### Impure Functions are Needed

Even understanding the importance of pure functions, if we stop to think about it, it's practically impossible to create a useful application without reading or writing to the "outside world". So, here's the following recommendation from the Scala documentation:

> Write the core of your application using pure functions, and then write an impure “wrapper” around that core to interact with the outside world.

### Key Points

- A pure function is a function that depends only on its declared inputs to produce its output.
- A pure function does not read or modify values from the "outside world".
- Real-world applications consist of a combination of pure and impure functions.

## Immutability

Mutable

```scala
scala> var y: Int = 10
var y: Int = 10

scala> y = 5
y: Int = 5
```

Immutable

```scala
scala> val x: Int = 10
val x: Int = 10

scala> x = 5
-- [E052] Type Error: ----------------------------------------------------------
1 |x = 5
  |^^^^^
  |Reassignment to val x

longer explanation available when compiling with `-explain`
1 error found
```

It is recommended to use `val` whenever possible.

### Key Points

## Higher-order Functions

## Closures

## Functional Composition

## Currying

> Currying is the technique of converting a function that takes multiple arguments into a sequence of functions that each takes a single argument

```scala
def add(x: Int)(y: Int): Int = x + y
```

```scala
scala> val add10 = add(10)
scala> add10(5)
val res0: Int = 15
```

## Map, Filter and Reduce

### Map

```scala
scala> val data: List[Int] = List(1, 2, 3, 4, 5)
val data: List[Int] = List(1, 2, 3, 4, 5)

scala> data.map(_ * 2)
val res0: List[Int] = List(2, 4, 6, 8, 10)
```

Combining currying with map:

```scala
scala> val data: List[Int] = List(1, 2, 3, 4, 5)
val data: List[Int] = List(1, 2, 3, 4, 5)

scala> def add(x: Int)(y: Int): Int = x + y
def add(x: Int)(y: Int): Int

scala> data.map(add(1))
val res0: List[Int] = List(2, 3, 4, 5, 6)
```

### Filter

```scala
scala> val data: List[Int] = List(1, 2, 3, 4, 5)
val data: List[Int] = List(1, 2, 3, 4, 5)

scala> data.filter(_ > 3)
val res0: List[Int] = List(4, 5)
```

### Reduce

```scala
scala> val data: List[Int] = List(1, 2, 3, 4, 5)
val data: List[Int] = List(1, 2, 3, 4, 5)

scala> data.reduce(_ + _)
val res15: Int = 15
```

### Mixing the Trio

Combining map, filter and reduce:

```scala
scala> val data: List[Int] = List(1, 2, 3, 4, 5)
val data: List[Int] = List(1, 2, 3, 4, 5)

scala> data.map(_ * 2).filter(_ > 5).reduce(_ + _)
val res18: Int = 24
```

## Referential Transparency

## Tail Recursion

```scala
scala> def gcd(a: Int, b: Int): Int = if (b == 0) a else gcd(b, a % b)
def gcd(a: Int, b: Int): Int

scala> gcd(21, 14)
val res0: Int = 7
```

Not tail recursion:

```scala
scala> def factorial(n: Int): Int = if (n == 0) 1 else n * factorial(n - 1)
def factorial(n: Int): Int

scala> factorial(5)
val res0: Int = 120
```

In scala we need to use the `tailrec`:

```scala
scala> import scala.annotation.tailrec
import scala.annotation.tailrec

scala> @tailrec
     | def gcd(a: Int, b: Int): Int = if (b == 0) a else gcd(b, a % b)
def gcd(a: Int, b: Int): Int

scala> gcd(21, 14)
val res0: Int = 7
```

If the `tailrec` is used in a function that does not implement the tail recursion it will raise an error:

```scala
scala> import scala.annotation.tailrec
import scala.annotation.tailrec

scala> @tailrec
     | def factorial(n: Int): Int = if (n == 0) 1 else n * factorial(n - 1)
       def factorial(n: Int): Int = if (n == 0) 1 else n * factorial(n - 1)
                                                         ^
On line 2: error: could not optimize @tailrec annotated method factorial: it contains a recursive call not in tail position
```

:warning: `tailrec` is just a check for the programmer to verify if the function will in fact be optimized. If the function already implement the tail recurssion (without `tailrec`) it will be automatically optimized in compilation time.

## Enumerations

## Anonymous Functions

## Option, Some, None

## Try, Success, Failure

## Companion Objects

## Case Classes

## Short-circuit Evaluation

## References

- [So You Want to be a Functional Programmer](https://cscalfani.medium.com/so-you-want-to-be-a-functional-programmer-part-1-1f15e387e536) (Charles Scalfani)
- [Scala Book](https://alvinalexander.com/scala/scala-book-free/) (Alvin Alexander, et al.)
- [Scala Documentation](https://docs.scala-lang.org/)
