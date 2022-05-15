# <img height=25 src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/scala/scala-original.svg" /> Functional Programming in Scala

This document aims to present a simple but useful introduction to the core concepts of functional programming with examples in Scala.
Although the examples are written in Scala, most of the concepts presented here are general to the functional programming paradigm and can be easily applied to other languages.

If you have any doubts or suggestions about the content, feel free to contribute opening an issue or a pull request.

> This document was created as a personal effort for studying Scala and functional programming and will likely have updates over time.

## Topics

- [Introduction](#introduction)
- [Immutability](#immutability)
- [Pure Functions](#pure-functions)
- [Higher-order Functions](#higher-order-functions)
- [Currying](#currying)
- [Anonymous Functions](#anonymous-functions)
- [Map, Filter and Reduce](#map-filter-and-reduce)
- [Tail Recursion](#tail-recursion)
- [Companion Objects](#companion-objects)
- [Substitution Model](#substitution-model)
- [Where to go next](#where-to-go-next)
- [References](#references)

## Introduction

Like many other modern programming languages, Scala is a multi-paradigm language which supports concurrent, functional, imperative and object-oriented paradigms. Despite that, it is important to note that this document will focus only on the functional paradigm.

Scala is designed to express common programming patterns in a concise, elegant, and type-safe way. Furthermore, Scala provides a lightweight syntax for defining anonymous functions, it supports higher-order functions, it allows functions to be nested, and it supports currying. Scala’s case classes and its built-in support for pattern matching provide the functionality of algebraic types, which are used in many functional languages. Also, singleton objects provide a convenient way to group functions that aren’t members of a class.

In the following sections you're going to find some practical examples about these functional programming concepts. Note that the examples can be reproduced in the Scala REPL.

## Immutability

In simple words, an immutable object is an object which cannot be modified after its creation.

As well as the other topics presented here, immutability is one of the core concepts in functional programming. Unlike other languages like Java, where you need to use the `final` keyword to make something immutable, in Scala we can simply use the keyword `val` instead of `var`, for example:

```scala
scala> var y: Int = 10
var y: Int = 10

scala> y = 5
y: Int = 5

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

Note that Scala throws a `Type Error` when trying to re-assign the `x` value.

Also, by default function parameters are immutable objects:

```scala
scala> def addOne(x: Int): Int = x = x + 1
-- [E052] Type Error: ----------------------------------------------------------
1 |def addOne(x: Int): Int = x = x + 1
  |                          ^^^^^^^^^
  |                          Reassignment to val x

longer explanation available when compiling with `-explain`
1 error found
```

Futhermore, Scala splits the collections hierarchy into "immutable" and "mutable" data structures. By default, Scala automatically puts the immutable data structures (such as Map and Set) into your default environment, so if you create a new Map or Set, you'll automatically get an immutable structure.

### Key Points

- By default Scala uses immutable objects (e.g. collections like Map and Set).
- Make your "variables" immutable, unless there’s a good reason not to.

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

Now, here is an example of an **impure function**:

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

As you can see, the `sum` function now depends on the `y` variable, which is in the outer scope. If you change the value of `y` it will affect the result of the `sum` function. So, even when calling the function with the same parameters it will return a different result.

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

Even understanding the importance of pure functions, when stopping to think about it, it's practically impossible to create a useful application without reading or writing to the "outside world". So, here's the following recommendation from the Scala documentation:

> Write the core of your application using pure functions, and then write an impure “wrapper” around that core to interact with the outside world.

### Key Points

- A pure function is a function that depends only on its declared inputs to produce its output.
- A pure function does not read or modify values from the "outside world".
- Real-world applications consist of a combination of pure and impure functions.

## Higher-order Functions

In short, higher-order functions are functions that can take other functions as arguments and/or return functions. All other functions are first-order functions. Here is an example of a higher-order function `calculate` that takes a function `func` as argument and uses it to do the calculation:

```scala
scala> def sum(x: Int, y: Int): Int = x + y
def sum(x: Int, y: Int): Int

scala> def mult(x: Int, y: Int): Int = x * y
def mult(x: Int, y: Int): Int

scala> def calculate(func: (Int, Int) => Int, x: Int, y: Int): Int = func(x, y)
def calculate(func: (Int, Int) => Int, x: Int, y: Int): Int

scala> calculate(sum, 2, 3)
val res0: Int = 5

scala> calculate(mult, 2, 3)
val res1: Int = 6
```

It is also possible to return functions, for example:

```scala
scala> def sum(x: Int, y: Int): Int = x + y
def sum(x: Int, y: Int): Int

scala> def mult(x: Int, y: Int): Int = x * y
def mult(x: Int, y: Int): Int

scala> def getFunction(functionName: String): (Int, Int) => Int = functionName match {
     |   case "sum"  => sum
     |   case "mult" => mult
     |   case _      => throw new Exception("Invalid function name")
     | }
def getFunction(functionName: String): (Int, Int) => Int

scala> val calculate: (Int, Int) => Int = getFunction("sum")
val calculate: (Int, Int) => Int = $Lambda$1087/0x000000080100b5d0@439f2d87

scala> calculate(2, 3)
val res0: Int = 5
```

As you will see later, some built-in functions like `.map()` and `.filter()` are great examples of higher-order functions.

### Key Points

- Higher-order functions can take one or more functions as arguments and/or return a function.
- Higher-order functions are used everywhere in functional programming.

## Currying

Currying is a technique of converting a function that takes multiple arguments into a sequence of functions that each takes a single argument. For example, a function `x=f(a, b)` would become two functions `y=g(a)` and `x=y(b)`, or called in sequence `x=g(a)(b)`.

In functional programming languages it provides a way of automatically managing how arguments are passed to functions, for example:

```scala
scala> def add(x: Int)(y: Int): Int = x + y
def add(x: Int)(y: Int): Int

scala> val add10: Int => Int = add(10)
val add10: Int => Int = Lambda$1390/37235503@236b4a44

scala> add10(15)
val res0: Int = 25
```

Note that it could also be represented as a closure:

```scala
scala> def add(x: Int): (Int) => Int = {
     |   def innerFunction(y: Int): Int = x + y
     |   innerFunction
     | }
def add(x: Int): Int => Int

scala> val add10: Int => Int = add(10)
val add10: Int => Int = $Lambda$1078/0x000000080100add8@536b71b4

scala> add10(15)
val res0: Int = 25
```

But using the currying syntax is much easier.

### Key Points

- Currying is a technique of converting a function that takes multiple arguments into a sequence of functions that each takes a single argument.
- Currying is very useful when you need to apply some initial values to many other values/objects.

## Anonymous Functions

An anonymous function (also known as lambda function) is a function definition that is not bound to an identifier. Anonymous functions are often used as arguments being passed to higher-order functions or used as a return value from higher-order functions that needs to return a function. If the function is only used once, or a limited number of times, an anonymous function may be syntactically lighter than using a named function.

```scala
scala> val data: List[Int] = List(1, 2, 3)
val data: List[Int] = List(1, 2, 3)

scala> def double(x: Int): Int = x * 2
def double(x: Int): Int

scala> data.map(double) // map using named function
val res0: List[Int] = List(2, 4, 6)

scala> data.map(x => x * 2) // map using anonymous function
val res1: List[Int] = List(2, 4, 6)

scala> data.map(_ * 2) // map using anonymous function (concise version)
val res2: List[Int] = List(2, 4, 6)
```

## Map, Filter and Reduce

The functions `.map()`, `.filter()` and `.reduce()` are well know functions and are used in many programming languages, but in functional programming they are even more important since it avoids loops and modifying values from the outer scope. It is important to note that these functions always return a new object.

### Map

The `.map()` function basically applies a function, passed as argument, to each value of a sequence of values, for example:

```scala
scala> val data: List[Int] = List(1, 2, 3, 4, 5)
val data: List[Int] = List(1, 2, 3, 4, 5)

scala> def double(x: Int): Int = x * 2
def double(x: Int): Int

scala> data.map(double)
val res0: List[Int] = List(2, 4, 6, 8, 10)

scala> data.map(_ * 2) // using anonymous functions
val res1: List[Int] = List(2, 4, 6, 8, 10)

scala> data
val res2: List[Int] = List(1, 2, 3, 4, 5)
```

In the example above it applies the `double` function to each element of the list and returns a new list object (as you can see in the last line, the original data was not modified).

Combining currying with map:

```scala
scala> val data: List[Int] = List(1, 2, 3, 4, 5)
val data: List[Int] = List(1, 2, 3, 4, 5)

scala> def add(x: Int)(y: Int): Int = x + y
def add(x: Int)(y: Int): Int

scala> data.map(add(3))
val res0: List[Int] = List(4, 5, 6, 7, 8)
```

### Filter

The `.filter()` function basically applies a function that returns a boolean, and is passed as argument, to each value of a sequence of values, for example:

```scala
scala> val data: List[Int] = List(1, 2, 3, 4, 5)
val data: List[Int] = List(1, 2, 3, 4, 5)

scala> data.filter(_ > 3)
val res0: List[Int] = List(4, 5)
```

As you can see the final result is a new list containing the objects that had a `true` assert when passing by the filter function (in this case, only values higher than 3).

### Reduce

The `.reduce()` function basically applies a function to each value and keeps an "accumulator" so it is used to reduce multiple values to a single one, for example:

```scala
scala> val data: List[Int] = List(1, 2, 3, 4, 5)
val data: List[Int] = List(1, 2, 3, 4, 5)

scala> def sum(acc: Int, value: Int): Int = {
     |   println(s"acc: $acc, value: $value")
     |   acc + value
     | }
def sum(acc: Int, value: Int): Int

scala> data.reduce(sum)
acc: 1, value: 2
acc: 3, value: 3
acc: 6, value: 4
acc: 10, value: 5
val res0: Int = 15

scala> data.reduce(_ + _)
val res1: Int = 15
```

### Mixing them all

```scala
scala> val data: List[Int] = List(1, 2, 3, 4, 5)
val data: List[Int] = List(1, 2, 3, 4, 5)

scala> data.map(_ * 2).filter(_ > 5).reduce(_ + _)
val res0: Int = 24
```

### Key Points

- Map, filter and reduce are always applied to every single value of a sequence.
- Map, filter and reduce are not hard to understand and can make your life much easier.
- Map, filter and reduce are used everywhere in functional programming.

## Tail Recursion

[This](https://stackoverflow.com/a/33930/4946821) great StackOverflow answer explains that in traditional recursion, the typical model is that you perform your recursive calls first, and then you take the return value of the recursive call and calculate the result. In this manner, you don't get the result of your calculation until you have returned from every recursive call.

In tail recursion, you perform your calculations first, and then you execute the recursive call, passing the results of your current step to the next recursive step. Basically, the return value of any given recursive step is the same as the return value of the next recursive call.

The consequence of this is that once you are ready to perform your next recursive step, you don't need the current stack frame anymore. This allows for some optimization. It will simply reuse the current stack frame for the next recursive step

The following example shows a function that, at first glance, looks like a tail recursive function but it is not because the last action multiplies a value of the current step with the recursive call:

```scala
scala> def factorial(n: Int): Int = {
     |   if (n <= 1) 1
     |   else n * factorial(n - 1)
     | }
def factorial(n: Int): Int

scala> factorial(5)
val res0: Int = 120
```

Now, the following exaple calculates the Greatest Common Divisor (GCD) using the tail recursion technique:

```scala
scala> def gcd(a: Int, b: Int): Int = {
     |   if (b == 0) a
     |   else gcd(b, a % b)
     | }
def gcd(a: Int, b: Int): Int

scala> gcd(21, 14)
val res0: Int = 7
```

Scala has an annotation called `tailrec` that helps in identifying functions that uses tail recursion, for example:

```scala
scala> import scala.annotation.tailrec
import scala.annotation.tailrec

scala> @tailrec
     | def gcd(a: Int, b: Int): Int = {
     |   if (b == 0) a
     |   else gcd(b, a % b)
     | }
def gcd(a: Int, b: Int): Int

scala> gcd(21, 14)
val res0: Int = 7
```

If the `tailrec` is used in a function that does not implement the tail recursion it will raise an error:

```scala
scala> import scala.annotation.tailrec
import scala.annotation.tailrec

scala> @tailrec
     | def factorial(n: Int): Int = {
     |   if (n <= 1) 1
     |   else n * factorial(n - 1)
     | }
         else n * factorial(n - 1)
                ^
On line 4: error: could not optimize @tailrec annotated method factorial: it contains a recursive call not in tail position
```

:warning: `tailrec` is just a check for the programmer to verify if the function will in fact be optimized. If the function already implement the tail recurssion (without `tailrec`) it will be automatically optimized in compilation time.

## Companion Objects

The _companion object_ concept is not exactly related to functional programming but is broadly used in Scala code.

A companion object in Scala is an `object` that is declared in the same file as a `class`, and has the same name as the class, for example:

```scala
class Settings {
  def showPath() = println(Settings.path)
}

object Settings {
  private val path = "tmp/settings.json"
}
```

And it is possible to use it as follows:

```scala
scala> new Settings().showPath()
tmp/settings.json
```

Besides that, companion objects have several benefits like:
- Companion objects can access each other's private members.
- Create new instances of a class without using the `new` keyword.
- Create multiple "constructors" (actually factory methods) for a class.
- Allows the creation of an `unapply` method.
- Allows the creation of functions that acts like "static methods".

More details about companion objects can be found in the [Scala Book page](https://docs.scala-lang.org/overviews/scala-book/companion-objects.html).

## Substitution Model

Scala uses the principle of substitution model to evaluate expressions. The idea is that all evaluation reduce an expression to a value so, for example, variable names are replaced by the values they are bound to. The expressions are evaluated in the same way we would evaluate a mathematical expression, for example:

```scala
scala> def x = 5
scala> def y = 10
scala> (3 * x) + (2 * y)
```

```
-> (3 * 5) + (2 * y)
-> 15 + (2 * 10)
-> 15 + 20
-> 35
```

Functions are evaluated in the same way as expressions, for example:

```scala
scala> def square(x: Double) = x * x
scala> square(2 + 3)
```

```
-> square(5)
-> 5 * 5
-> 25
```

But in function evaluation there are two strategies called `call-by-name` and `call-by-value`. Here is an example of how both work:

```scala
scala> def square(x: Double) = x * x
scala> def sumOfSquares(x: Int, y: Int) = square(x) + square(y)
```

- `call-by-value`: evaluates every function argument only once thus it avoids the repeated evaluation of arguments.

```
-> sumOfSquares(2, 2 + 3)
-> sumOfSquares(2, 5)
-> square(2) + square(5)
-> 2 * 2 + 5 * 5
-> 4 + 25
-> 29
```

- `call-by-name`: avoids evaluation of parameters if it is not used in the function body.

```
-> sumOfSquares(2, 2 + 3)
-> square(2) + square(2 + 3)
-> 2 * 2 + square(2 + 3)
-> 4 + (2 + 3) * (2 + 3)
-> 4 + 5 * (2 + 3)
-> 4 + 5 * 5
-> 4 + 25
-> 29
```

As you can see, `call-by-value` is more efficient then `call-by-name` so Scala uses `call-by-value` as the default evaluation strategy, however you can force it to use the `call-by-name` strategy using the follwing syntax `=>`, for example:

```scala
scala> def loop: Int = loop
scala> def test(x: Int, y: => Int) = x
scala> test(1, loop) // infinite loop
```

The above example uses `call-by-value` and enter the infinite loop.

Now the same example using `call-by-name`:

```scala
scala> def loop: Int = loop
scala> def test(x: Int, y: => Int) = x
scala> test(1, loop)
val res0: Int = 1
```

As you can see it evaluates only the `x` value which is being used inside the function and returns it.

More details about Scala substitution model can be found in [this article](http://bkpathak.github.io/scala-substitution-model#:~:text=Scala%20model%20of%20expression%20evaluation,does%20not%20need%20further%20evaluation.).

### Key Points

- Scala uses the `call-by-value` strategy by default.
- It is important to understand the difference between `call-by-name` and `call-by-value` but in most cases you don't need to worry about it.

## Where to go next

If you are completely new to Scala, I really recommend the [Scala Book](https://alvinalexander.com/scala/scala-book-free/), it is a relatively short book and covers the most important topics related to the language.

Another great source of knowledge for beginners is the Udemy course "[Scala at Light Speed](https://www.udemy.com/course/fast-scala/)", which is a quick course (~2h) that will give you an overview about the language and its syntax.

Last but not least, [Functional Programming: Simplified](https://www.amazon.com.br/dp/B076J7CJKY/ref=dp-kindle-redirect?_encoding=UTF8&btkr=1) is another excellent book that covers many topics related to functional programming with examples in Scala.

## References

- [So You Want to be a Functional Programmer](https://cscalfani.medium.com/so-you-want-to-be-a-functional-programmer-part-1-1f15e387e536) (Charles Scalfani)
- [Scala Book](https://alvinalexander.com/scala/scala-book-free/) (Alvin Alexander, et al.)
- [Scala Documentation](https://docs.scala-lang.org/)
- [Anonymous Functions](https://en.wikipedia.org/wiki/Anonymous_function)
- [Scala Substitution Model](http://bkpathak.github.io/scala-substitution-model#:~:text=Scala%20model%20of%20expression%20evaluation,does%20not%20need%20further%20evaluation.) (Bijay Pathak)
- [Scala at Light Speed](https://www.udemy.com/course/fast-scala/) (Rock the JVM)
