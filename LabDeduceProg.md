# Lab: Linked Lists

## Overview

For this lab, you will write and test several functions with linked
lists in the Deduce programming language.

### Table of Contents

1. [Deduce Setup](#deduce-setup)
2. **[Problem Set](#problem-set)**

## Deduce Setup

### Installation

You can find the code for deduce
[here](https://github.com/jsiek/deduce), as well as instructions on
running your code.

You also need to install [python](https://www.python.org/), if you
haven't already. Here are some
[instructions](https://wiki.python.org/moin/BeginnersGuide/Download)
and links to the download for various systems.

To install the requisite packages for deduce, run the following
command in the same directory as `deduce.py`.

```bash
python -m pip install lark
```

### Running a proof file

Create a file in the same directory as `deduce.py` called `hello.pf`,
and paste the following into it.

```
import Nat

define hello = 42

print hello
```

Then run the command

```bash
python ./deduce.py ./hello.pf
```

and you should see the following output.

```
42
hello.pf is valid
```

## Problem Set

### Sum the elements of a List

Define a function named `sum` that adds up the elements of a list.
In particular, the elements are natural numbers, so they have type
`Nat`. The `sum` function should have the following type.

```
sum : fn List<Nat> -> Nat
```

You will need to import the type `Nat` and operations, such as
`operator +`, from `Nat.pf`, with the following `import` statement.

```
import Nat
```

The following shows an example use of the `sum` function.

```
assert sum([1,2,3,4]) = 10
```

### Concatenating a List of Lists

Define a function named `concat` that turns a list-of-lists into a
list. The `concat` function should have the following type.

```
concat : < E > fn List<List<E>> -> List<E>
```

In general, you may use any functions in `List.pf`.
The following shows an example use of the `concat` function.

```
assert concat([[1,2,3], [4,5]]) = [1,2,3,4,5]
```

Use this `assert` statement and several of your own to test whether
your `concat` function behaves as expected.


### Quick Reverse, Accumulator-Passing Style

The `reverse` function in `List.pf` is `O(n²)` time because it invokes
append (`operator ++`) `n` times and append is `O(n)`. Define a
function named `quick_rev` that reverses the input list and that is
`O(n)` time. The `quick_rev` function should be generic and have the
following type.

```
quick_rev : < E > fn List<E> -> List<E>
```

Use `assert` statements to test whether your `quick_rev` function
really reverses the input.

Hint: we recommend that you define an auxilliary function that is
written in accumulator-passing style.

Consider the `sum` function that you created above.  We can change
`sum` to accumulator-passing style by adding an extra parameter that
stores the total-so-far.

```
function sum_accum(List<Nat>, Nat) -> Nat {
  sum_accum(empty, total) = total
  sum_accum(node(x, xs), total) = sum_accum(xs, x + total)
}
```

(One side benefit of accumulator passing style is that the function is
tail recursive, which means that it uses `O(1)` space on the procedure
call stack, whereas `sum` uses `O(n)` space.)

```
assert sum([1,2,3]) = sum_accum([1,2,3], 0)
```

### Cumulative Sum of a List

The **cumulative sum** of a list of numbers produces a list where each
element is the sum of all the elements of the input list up to and
including the index of the current element.
So if the input list is 
```
n0, n1, n2, ...
```
the output list is
```
n0, n0+n1, n0+n1+n2, ...
```

Define a function named `cumulative_sum` that performs this operation.
The `cumulative_sum` function should have the following type.

```
cumulative_sum : List<Nat> -> List<Nat>
```

Here is an example of using the `cumulative_sum` function.

```
assert cumulative_sum([3,1,5,2,4]) = [3,4,9,11,15]
```

Test your `cumulative_sum` function with several more `assert`
statements.

