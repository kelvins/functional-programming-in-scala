# <img height=25 src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/scala/scala-original.svg" /> Functional Programming in Scala

This document aims to present a simple but useful introduction to the core concepts of functional programming with examples in Scala.
Although the examples are written in Scala, most of the concepts presented here are general to the functional programming paradigm and can be easily applied to other languages.

If you have any doubts or suggestions about the content, feel free to contribute opening an issue or a pull request.

## Topics

- [Introduction](#introduction)
- [Substitution Model](#substitution-model)
- [Pure Functions](#pure-functions)
- [Immutability](#immutability)
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
- [References](#references)

## Introduction

Like many other modern programming languages, Scala is a multi-paradigm language which supports concurrent, functional, imperative and object-oriented paradigms. Despite that, in this document we're going to focus only in the functional paradigm.

## Substitution Model

## Pure Functions

According to [Wikipedia](https://en.wikipedia.org/wiki/Pure_function), pure functions are functions that have the following properties:

- The function **return value will always be the same** for the same input arguments;
- The function **has no side effects**.

Thus a pure function is a computational analogue of a mathematical function.

Pure Functions:

```scala
def sum(x: Int, y: Int): Int = x + y
```

```scala
scala> sum(10, 5)
val res0: Int = 15

scala> sum(10, 5)
val res1: Int = 15
```

Impure Functions:

Side effect:

```scala
var y: Int = 5

def sum(x: Int): Int = {
  x + y
}
```

```scala
scala> sum(5)
val res0: Int = 10

scala> y = 10
y: Int = 10

scala> sum(5)
val res1: Int = 15
```

Random value:

```scala
def sum(x: Int, y: Int): Int = {
  val rand: Int = scala.util.Random.nextInt(100)
  x + y + rand
}
```

```scala
scala> sum(10, 5)
val res0: Int = 73

scala> sum(10, 5)
val res1: Int = 110
```

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
def sum(data: List[Int]): Int = {
  if data.isEmpty then 0 else data.head + sum(data.tail)
}
```

```scala
scala> val data: List[Int] = List(1, 2, 3, 4, 5)
val data: List[Int] = List(1, 2, 3, 4, 5)

scala> sum(data)
val res0: Int = 15
```

## Enumerations

## Anonymous Functions

## Option, Some, None

## Try, Success, Failure

## Companion Objects

## Case Classes

## References

- [So You Want to be a Functional Programmer](https://cscalfani.medium.com/so-you-want-to-be-a-functional-programmer-part-1-1f15e387e536) (Charles Scalfani)
- [Scala Book](https://alvinalexander.com/scala/scala-book-free/) (Alvin Alexander, et al.)
