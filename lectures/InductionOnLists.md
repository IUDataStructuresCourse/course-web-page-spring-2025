# Proof by Induction

Concepts:
* `induction` on lists

Example:
```{.deduce^#list_append_empty}
theorem list_append_empty: <U> all xs :List<U>.
  xs ++ [] = xs
proof
  arbitrary U:type
  induction List<U>
  case [] {
    conclude @[]<U> ++ [] = []  by definition operator++
  }
  case node(n, xs') assume IH: xs' ++ [] = xs' {
    equations
          node(n, xs') ++ []
        = node(n, xs' ++ [])    by definition operator++
    ... = node(n, xs')          by rewrite IH
  }
end
```

Concepts:
* `induction` on natural numbers

Example:
```{.deduce^#take_drop_append_1}
theorem take_drop_append_1:  <T> all n:Nat, xs:List<T>.
  if n ≤ length(xs)
  then take(n, xs) ++ drop(n, xs) = xs
proof
  arbitrary T:type
  induction Nat
  case 0 {
    arbitrary xs:List<T>
    assume prem
    conclude take(0, xs) ++ drop(0, xs) = xs
        by evaluate
  }
  case suc(n') assume IH {
    arbitrary xs:List<T>
    switch xs {
      case [] {
        assume premise: suc(n') ≤ length(@[]<T>)
        have eq: suc(n') ≤ 0 by definition length in premise
        conclude false by definition operator≤ in eq
      }
      case node(x, xs') {
        assume prem: suc(n') ≤ length(node(x, xs'))
        have eq: 1 + n' ≤ 1 + length(xs') by rewrite suc_one_add[n'] in definition length in prem
        have: n' ≤ length(xs') by apply less_equal_left_cancel to eq
        equations
              take(suc(n'), node(x, xs')) ++ drop(suc(n'), node(x, xs')) 
            = node(x, take(n', xs')) ++ drop(n', xs')  by definition {take, drop}
        ... = node(x, take(n', xs') ++ drop(n', xs'))  by definition operator++
        ... = node(x, xs')                             by rewrite (apply IH to recall n' ≤ length(xs'))
      }
    }
  }
end
```

The proof also goes through if we instead do induction on the list `xs`.
(Skip over this proof.)

```{.deduce^#take_drop_append_2}
theorem take_drop_append_2:  <T> all xs:List<T>, n:Nat.
  if n ≤ length(xs)
  then take(n, xs) ++ drop(n, xs) = xs
proof
  arbitrary T:type
  induction List<T>
  case [] {
    arbitrary n:Nat
    assume prem: n ≤ length(@[]<T>)
    have: n ≤ 0                           by definition length in prem
    have: n = 0                           by apply zero_le_zero to recall n ≤ 0
    suffices take(0, @[]<T>) ++ drop(0, []) = []  by rewrite recall n = 0
    suffices @[]<T> ++ [] = []                    by definition {drop, take}
    definition operator++
  }
  case node(x, xs) assume IH {
    arbitrary n:Nat
    switch n for take, drop {
      case 0 assume n_z {
        assume prem: 0 ≤ length(node(x, xs))
        conclude @[]<T> ++ node(x, xs) = node(x, xs)   by definition operator++
      }
      case suc(n') {
        assume prem
        have: n' ≤ length(xs)
            by definition {length, operator+, operator+, operator≤} in prem
        equations
              node(x, take(n', xs)) ++ drop(n', xs) 
            = node(x, take(n', xs) ++ drop(n', xs))
                  by definition operator++
        ... = node(x, xs) by rewrite (apply IH to recall n' ≤ length(xs))
      }
    }
  }
end
```

Concepts:
* The types of mathematical sets: `Set`
* `set_of` converts a list to a set
* `single(x)` creates a set with one element
* `∪` is set union
* There are many theorems about sets in `lib/Set.pf`, such as
  `empty_union` and `union_assoc`.

Example:
```{.deduce^#setof_append}
theorem setof_append: <T> all xs:List<T>, ys:List<T>.
  set_of(xs ++ ys) = set_of(xs) ∪ set_of(ys)
proof
  arbitrary T:type
  induction List<T>
  case [] {
    arbitrary ys:List<T>
    suffices set_of(ys) = ∅ ∪ set_of(ys)   by definition {operator++, set_of}
    rewrite empty_union<T>
  }
  case node(x, xs') assume IH {
    arbitrary ys:List<T>
    equations
          set_of(node(x, xs') ++ ys) 
        = single(x) ∪ set_of(xs' ++ ys)           by definition {operator++, set_of}
    ... = single(x) ∪ (set_of(xs') ∪ set_of(ys))  by rewrite IH
    ... = (single(x) ∪ set_of(xs')) ∪ set_of(ys)  by symmetric union_assoc<T>
    ... = #set_of(node(x, xs'))# ∪ set_of(ys)     by definition set_of
  }
end
```

<!--
```{.deduce^file=InductionOnLists.pf}
import Nat
import List
import Set

<<list_append_empty>>
<<take_drop_append_1>>
<<take_drop_append_2>>
<<setof_append>>
```
-->
