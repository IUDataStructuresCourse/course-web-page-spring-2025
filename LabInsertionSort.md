# Lab: Insertion Sort (Tail Recursive)

The `insertion_sort` function that we discussed in lecture uses more
computer memory than necessary because it uses one frame on the
procedure call stack for every element in the input list. This can be
avoided if we instead implement Insertion Sort using a tail-recursive
function. That is, as a function that immediately returns after the
recursive call. For this lab, formulate a tail recursive version of
Insertion Sort, test it, and prove that it is correct.

As a hint, define an auxiliary function `isort(xs,ys)` that takes a
list xs and an already sorted list ys and returns a sorted list that
includes the contents of both xs and ys. This is another example
of accumulator-passing style, where `ys` is the accumulator

```
function isort(List<Nat>, List<Nat>) -> List<Nat> {
  FILL IN HERE
}
```

To implement `isort`, use the `insert` auxilliary function that we
used to implement `insertion_sort`.

```
function insert(List<Nat>,Nat) -> List<Nat> {
  insert(empty, x) = node(x, empty)
  insert(node(y, next), x) =
    if x ≤ y then
      node(x, node(y, next))
    else
      node(y, insert(next, x))
}
```

Once you have defined `isort`, you can implement Insertion Sort as follows.

```
define insertion_sort2 : fn List<Nat> -> List<Nat>
    = λxs{ isort(xs, empty) }
```

To prove the correctness of `insertion_sort2`, prove that the result is sorted

```
theorem insertion_sort2_sorted: all xs:List<Nat>. 
  sorted( insertion_sort2(xs) )
proof
  ?
end
```

Here is the definition of `sorted`:

```
function sorted(List<Nat>) -> bool {
  sorted(empty) = true
  sorted(node(a, next)) =
    sorted(next) and all_elements(next, λb{ a ≤ b })
}
```

You may reuse theorems from the proof of correctness for `insertion_sort`.
In particular, you will need `less_equal_insert` and `insert_sorted`.
These theorems and the definitions of `insert`  and `sorted` are
in the file [InsertionSortStarter.pf](./InsertionSortStarter.pf).


## Extra Credit

Prove that the output includes all of the same elements as in the
input (the correct number of times).  You may use `insert_contents`
from the proof for `insertion_sort_contents` (also in the starter file).

```
theorem insertion_sort2_contents: all xs:List<Nat>. 
  mset_of( insertion_sort2(xs) ) = mset_of(xs)
proof
  ?
end
```
