# Induction on Lists

```{.deduce^#take_drop_append}
theorem take_drop_append:  <T> all xs:List<T>, n:Nat.
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
        assume prem
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

<!--
```{.deduce^file=InductionOnLists.pf}
import Nat
import List

<<take_drop_append>>
```
-->
