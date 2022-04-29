# <img height=25 src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/scala/scala-original.svg" /> Functional Programming in Scala

This repository aims to present simple but useful content related to the core concepts of functional programming with examples in Scala.
Although the examples are written in Scala, the concepts presented here are general to the functional programming paradigm and can be easily applied to other languages.

## Topics

- [Introduction](#introduction)
- [Pure Functions](#pure-functions)
- [Immutability](#immutability)
- [Higher-order Functions](#higher-order-functions)
- [Closures](#closures)
- [Functional Composition](#functional-composition)
- [Currying](#currying)
- [Map](#map)
- [Filter](#filter)
- [Reduce](#reduce)
- [Referential Transparency](#referential-transparency)
- [Tail Recursion](#tail-recursion)
- [References](#references)

## Introduction

Like many modern programming languages, Scala is a multi-paradigm language which supports concurrent, functional, imperative and object-oriented paradigms.
Despite that, in these examples we're going to focus only in the functional paradigm.

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

## Map

## Filter

## Reduce

## Referential Transparency

## Tail Recursion

## References

- [So You Want to be a Functional Programmer](https://cscalfani.medium.com/so-you-want-to-be-a-functional-programmer-part-1-1f15e387e536)
