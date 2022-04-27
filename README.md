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

## Pure Functions

According to [Wikipedia](https://en.wikipedia.org/wiki/Pure_function), pure functions are functions that has the following properties:

- The function **return value will always be the same** for the same input arguments;
- The function **has no side effects**.

Thus a pure function is a computational analogue of a mathematical function.

Pure Functions:

```scala
def sum(x: Int, y: Int): Int = x + y
```

Impure Functions:

Side effect:

```scala
var y: Int = 5

def sum(x: Int): Int = {
  x + y
}
```

Random value:

```scala
def sum(x: Int, y: Int): Int = {
  val rand: Int = scala.util.Random.nextInt(100)
  x + y + rand
}
```

## Immutability

## Higher-order Functions

## Closures

## Functional Composition

## Currying

## Map

## Filter

## Reduce

## Referential Transparency

## Tail Recursion

## References

- [So You Want to be a Functional Programmer](https://cscalfani.medium.com/so-you-want-to-be-a-functional-programmer-part-1-1f15e387e536)
