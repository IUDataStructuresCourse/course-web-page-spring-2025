# Introduction to Deduce

The Deduce system has two parts: a programming language and a proof
language. The user of Deduce writes some functions in the Deduce
programming language and then writes proofs in the Deduce proof
language about the correctness of their funnctions. The Deduce system
checks that the proofs are correct.

In this lecture we introduce the Deduce programming language.

# Programming in Deduce

The main way to represent data in Deduce is with `union` types. 

## A Fruitful Example

The following statement defines a union type named `Fruit` and it
defines three kinds of fruit: apples, oranges, and bananas.

```{.deduce^#Fruit}
union Fruit {
  apple
  orange
  banana
}
```

We can create fruit values by mentioning one of the alternatives.  The
following statements put an apple into variable `f1` and a banana into
`f2`.

```{.deduce^#apple}
define f1 : Fruit = apple
define f2 : Fruit = banana
```

Of course, we all know that bananas tend to go rotten quickly, so we
should keep track of that information. We can accomplish that by
adding a parameter of type `bool` to the `banana` alternative:

```{.deduce^#Fruit2}
union Fruit {
  apple
  orange
  banana(bool)
}
```

and now when we create a banana, we have to say whether it is rotten
or not.

```{.deduce^#banana}
define f3 : Fruit = banana(false)
define f4 : Fruit = banana(true)
```

We can inspect and dispatch on values of union type using Deduce's
`switch` statement. The following statement inspects the fruit `f4`
and figures out whether it is rotten or not. In the case for `banana`,
Deduce initializes the `r` variable to `true` (because `f4` is
`banana(true)`).


```{.deduce^#switchFruit}
define r4 : bool = 
    switch f4 {
      case apple { false }
      case orange { false }
      case banana(r) { r }
    }
```

## Assert

To check whether the result of an term produces `true`, use the `assert` statement.
It will halt with an error if the term produces `false`.

```{.deduce^#assertR4}
assert r4
```

## Linked Lists via unions

A **linked list** is a data structure that represents a sequence of
elements.  Each element is stored inside a node and each node also
stores a link to the next node, or to the special empty value that
signifies the end of the list. The following is a linked list
that represents the sequence `7, 4, 5`.

```
   ___      ___      ___
  | 7 |--> | 4 |--> | 5 |-/
   ---      ---      ---
```


In Deduce we can define the type of linked lists of natural numbers
with the following `union` type. (`Nat` is the type of natural numbers
and is defined in `lib/Nat.pf`.)

```{.deduce^#NatList}
union NatList {
  Empty
  Node(Nat, NatList)
}
```

Then we can create the linked list for `7, 4, 5` with the following
statement that creates three nodes.

```{.deduce^#NatListL}
define L = Node(7, Node(4, Node(5, Empty)))
```

## Recursive Functions: List Length

The `len` function, defined below returns the number of elements in
a linked list. The length of an empty list is `0` and the length of a
list that starts with a node is one more than the length of the list
starting at the next node.

```{.deduce^#len}
function len(NatList) -> Nat {
  len(Empty) = 0
  len(Node(n, next)) = 1 + len(next)
}
```

The length of list `L` defined above is `3`.

```{.deduce^#lenL3}
assert len(L) = 3
```

Recursive functions in Deduce are somewhat special to make sure they
always terminate.

* The first parameter of the function must be a union.
* The function definition must include a clause for every
  constructor in the union.
* The first argument of every recursive call must be a sub-part of the
  current constructor of the union.


## Generic Unions

One often needs to create lists with other kinds of elements, not just
`Nat`.  Thus, Deduce supports generic unions. Here is the generic
`List` type defined in `lib/List.pf`.

```{.deduce^#List}
union List<T> {
  empty
  node(T, List<T>)
}
```

For example, the sequence of numbers `1, 2, 3` is represented
by the following linked list.

```{.deduce^#List123}
define list_123 : List<Nat> = node(1, node(2, node(3, empty)))
```

As shorthand for a sequence of `node` with `empty` at the end, there
is square-bracket syntax for creating a list.

```{.deduce^#List456}
define list_456 = [4,5,6]
```

Because `List` is generic, we can create a list that contains lists.

```{.deduce^#List123456}
define list_of_list = [list_123, list_456]
```

## Generic Recursive Functions

From `lib/List.pf`:

```{.deduce^#length}
function length<E>(List<E>) -> Nat {
  length(empty) = 0
  length(node(n, next)) = 1 + length(next)
}
```

## If-Then-Else

Deduce provides an if-then-else term that branches on the value of a
boolean. For example, the following if-then-else term is evaluates to
`42`.

```{.deduce^#assertIfTrue}
print if length([1,2,3]) = 3 then 42 else 0
```

## Linear Search

The following `search` function returns the position in the list of
the first occurence of the given number.

```{.deduce^#search}
function search(List<Nat>, Nat) -> Nat {
  search(empty, y) = 0
  search(node(x, xs), y) =
    if x = y then 0
    else 1 + search(xs, y)
}
```

If the given number is not in the list, then `search` returns an index
that is one-past the last element of the list.

```{.deduce^#searchExamples}
assert search(list_123, 1) = 0
assert search(list_123, 2) = 1
assert search(list_123, 3) = 2
assert search(list_123, 4) = 3
```

The time complexity of `search` is O(n) where n is the length of the
list.  This is because the depth of the recursion is n (in the worst
case) and the `search` function does at most a constant amount of work
in each call (the comparison `x = y`, the `if`, and the addition `1 +`).


## Import

The `import` declaration makes available the contents of another
Deduce file in the current file. For example, you can import the
contents of `lib/Nat.pf` as follows

```{.deduce^#importNat}
import Nat
```

If you need to import a file from some other directory, use
the `--dir directory-name` flag with `deduce.py` to specify
that other directory.

## Printing Values

You can ask Deduce to print a value to standard output using the
`print` statement.

```{.deduce^#printFive}
print 5
```

The output is `5`.


## Functions (`fun`) (non-recursive)

Functions are created with an term that starts with the `fun`
keyword, followed by parameter names and their types, then the body of
the function enclosed in braces.  For example, the following defines a
function for computing the area of a rectangle.

```{.deduce^#area}
define area = fun h : Nat, w : Nat { h * w }
```

To call a function, apply it to the appropriate number and type of
arguments.

```{.deduce^#area12}
assert area(3, 4) = 12
```

## Pairs

The `Pair` type is define in `lib/Pair.pf` as a generic union:

```{.deduce^#Pair}
union Pair<T,U> {
  pair(T,U)
}
```

The `first` function returns the first element of the pair
and `second` returns the second element.

```{.deduce^#firstSecond}
function first<T,U>(Pair<T,U>) -> T {
  first(pair(x,y)) = x
}

function second<T,U>(Pair<T,U>) -> U {
  second(pair(x,y)) = y
}
```

The following is an example of creating a pair and then accessing its
first and second elements.

```{.deduce^#firstPair}
assert first(pair(3,7)) = 3
assert second(pair(3,7)) = 7
```

## Generic Functions (`generic`) (non-recursive)

Generic non-recursive functions are created with a combination of
`generic` and `fun`.  Start with the `generic` keyword, followed by
the list of type parameters, then an open brace, then the function
(`fun`), and finally a close brace.

```{.deduce^#swap}
define swap = generic T, U { fun p:Pair<T,U> { pair(second(p), first(p)) } }

assert first(swap(pair(1,2))) = 2
assert second(swap(pair(1,2))) = 1
```


<!--
## Functions Returning Functions: nth element of list

The `nth` function retrieves the element at position `i` in list
`xs`. However, if `i` is greater or equal to the length of `xs`, then
`nth` returns the default value `d`.

```
function nth<T>(List<T>, T) -> (fn Nat -> T) {
  nth(empty, d) = fun i { d }
  nth(node(x, xs), d) = fun i {
    if i = 0 then
      x
    else
      nth(xs, d)(pred(i))
  }
}
```

(The `pred(n)` function is short for predecessor and computes `n - 1`,
except that `pred(0) = 0`.)

Here are examples of applying `nth` to the list `1, 2, 3`,
using `0` as the default value.

```
assert nth(list_123, 0)(0) = 1
assert nth(list_123, 0)(1) = 2
assert nth(list_123, 0)(2) = 3
assert nth(list_123, 0)(3) = 0
```

We formulated the `nth` operation in an unusual way. It has two
parameters and returns a function of one parameter that returns an
element. We could have instead made `nth` take three parameters and
directly return an element. We made this design choice because it
means we can use `nth` with several other functions and theorems that
work with functions of the type `fn Nat -> T`.
-->

<!--
```{.deduce^file=DeduceProgramming1.pf}
<<importNat>>
<<Fruit>>
<<apple>>
<<NatList>>
<<NatListL>>
<<len>>
<<lenL3>>
<<printFive>>
<<area>>
<<area12>>
```

```{.deduce^file=DeduceProgramming2.pf}
<<importNat>>
<<Fruit2>>
<<banana>>
<<switchFruit>>
<<assertR4>>
<<List>>
<<List123>>
<<List456>>
<<List123456>>
<<length>>
<<assertIfTrue>>
<<search>>
<<searchExamples>>
<<Pair>>
<<firstSecond>>
<<firstPair>>
<<swap>>
```
-->
