Concepts:
* [proof instantiation](https://jsiek.github.io/deduce/pages/reference.html#instantiation-proof)
* [`rewrite`](https://jsiek.github.io/deduce/pages/reference.html#rewrite-proof)

Example:
```{.deduce^#len_42}
theorem len_42:  1 = len(Node(42, Empty))
proof
  rewrite len_one[42]
end
```

Concepts:
* Library of theorems about natural numbers (`lib/Nat.thm`)
* [`equations`](https://jsiek.github.io/deduce/pages/reference.html#equations)

Example:
```{.deduce^#algebra_example}
theorem algebra_example: all x:Nat. (1 + x) * x = x * x + x
proof
  arbitrary x:Nat
  equations
        (1 + x) * x
      = x * (1 + x)        by mult_commute
  ... = x * 1 + x * x      by dist_mult_add
  ... = x * x + x * 1      by add_commute
  ... = x * x + x          by rewrite mult_one
end
```

Concepts:
* [`true`](https://jsiek.github.io/deduce/pages/reference.html#true-formula) formula
* [period](https://jsiek.github.io/deduce/pages/reference.html#period-proof-of-true)
* [`false`](https://jsiek.github.io/deduce/pages/reference.html#false) formula
* [`conclude`](https://jsiek.github.io/deduce/pages/reference.html#conclude-proof) proof

Example:
```{.deduce^#prove_true}
theorem prove_true: true
proof
  .
end
```

Example:
```{.deduce^#false_explosion}
theorem false_explosion: if false then 0 = 1
proof
  assume: false
  conclude 0 = 1 by recall false
end
```

Concepts:
* [`if`-`then`](https://jsiek.github.io/deduce/pages/reference.html#if-then-conditional-formula) formula
* [`have`](https://jsiek.github.io/deduce/pages/reference.html#have-proof-statement) proof
* [`apply`-`to`](https://jsiek.github.io/deduce/pages/reference.html#apply-to-proof-modus-ponens) proof
* [`recall`](https://jsiek.github.io/deduce/pages/reference.html#recall-proof) proof

From `lib/Nat.thm`:
```
max_equal_greater_right: all x:Nat, y:Nat. if x ≤ y then max(x, y) = y
```

Example:
```{.deduce^#modus_ponens_example}
theorem modus_ponens_example: all x:Nat. max(x, 1 + x) = 1 + x
proof
  arbitrary x:Nat
  have: x ≤ 1 + x   by less_equal_add_left
  conclude max(x, 1 + x) = 1 + x  by apply max_equal_greater_right to recall x ≤ 1 + x
end
```

Concepts:
* [`assume`](https://jsiek.github.io/deduce/pages/reference.html#assume) proof
* [`switch`](https://jsiek.github.io/deduce/pages/reference.html#switch-proof) proof
* [`rewrite`-`in`](https://jsiek.github.io/deduce/pages/reference.html#rewrite-in-proof)
* [`definition`-`in`](https://jsiek.github.io/deduce/pages/reference.html#definition-in-proof)

Example:
```{.deduce^#assume_example}
theorem assume_example: all xs:NatList. if len(xs) = 0 then xs = Empty
proof
  arbitrary xs:NatList
  assume premise: len(xs) = 0
  switch xs {
    case Empty {
      .
    }
    case Node(x, xs') assume xs_node: xs = Node(x, xs') {
      have len_zero: len(Node(x, xs')) = 0 by rewrite xs_node in premise
      conclude false by definition {len, operator+} in len_zero
    }
  }
end
```

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

<!--
```{.deduce^file=DeduceIntroProof2.pf}
import Nat
import DeduceProgramming1
import DeduceIntroProof
import Set
import List

<<len_42>>
<<algebra_example>>
<<modus_ponens_example>>
<<assume_example>>
<<prove_true>>
<<false_explosion>>
<<list_append_empty>>
```
-->

