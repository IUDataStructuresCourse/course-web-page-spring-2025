# Writing Proofs in Deduce

Concepts:
* [`theorem`](https://jsiek.github.io/deduce/doc/Reference.html#theorem-statement) statement
* [`definition`](https://jsiek.github.io/deduce/doc/Reference.html#definition-proof) proof

Example:
```{.deduce^#len_empty}
theorem len_empty: 0 = len(Empty)
proof
  definition len
end
```

Concepts:
* [`all`](https://jsiek.github.io/deduce/doc/Reference.html#all-universal-quantifier) formula,
* [`arbitrary`](https://jsiek.github.io/deduce/doc/Reference.html#arbitrary-forall-introduction) proof,
* [`suffices`](https://jsiek.github.io/deduce/doc/Reference.html#suffices-proof-statement) proof, and
* [`evaluate`](https://jsiek.github.io/deduce/doc/Reference.html#evaluate) proof.

Example:
```{.deduce^#len_one}
theorem len_one: all x:Nat. len(Node(x, Empty)) = 1
proof
  arbitrary x:Nat
  suffices 1 + 0 = 1 by definition {len, len}
  evaluate
end
```

Concepts:
* [proof instantiation](https://jsiek.github.io/deduce/doc/Reference.html#instantiation-proof)
* [`rewrite`](https://jsiek.github.io/deduce/doc/Reference.html#rewrite-proof)

Example:
```{.deduce^#len_42}
theorem len_42:  1 = len(Node(42, Empty))
proof
  rewrite len_one[42]
end
```

Concepts:
* Library of theorems about natural numbers (`lib/Nat.thm`)
* [`equations`](https://jsiek.github.io/deduce/doc/Reference.html#equations)

Example:
```{.deduce^#algebra_example}
theorem algebra_example: all x:Nat. (1 + x) * x = x * x + x
proof
  arbitrary x:Nat
  equations
        (1 + x) * x
      = x * (1 + x)        by mult_commute[1+x,x]
  ... = x * 1 + x * x      by dist_mult_add[x,1,x]
  ... = x * x + x * 1      by add_commute[x*1,x*x]
  ... = x * x + x          by rewrite mult_one[x]
end
```

Concepts:
* [`if`-`then`](https://jsiek.github.io/deduce/doc/Reference.html#if-then-conditional-formula) formula
* [`have`](https://jsiek.github.io/deduce/doc/Reference.html#have-proof-statement) proof
* [`apply`-`to`](https://jsiek.github.io/deduce/doc/Reference.html#apply-to-proof-modus-ponens) proof
* [`recall`](https://jsiek.github.io/deduce/doc/Reference.html#recall-proof) proof
* [`conclude`](https://jsiek.github.io/deduce/doc/Reference.html#conclude-proof) proof

From `lib/Nat.thm`:
```
max_equal_greater_right: (all x:Nat, y:Nat. (if x ≤ y then max(x, y) = y))
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
* [`assume`](https://jsiek.github.io/deduce/doc/Reference.html#assume) proof
* [`switch`](https://jsiek.github.io/deduce/doc/Reference.html#switch-proof) proof
* [`rewrite`-`in`](https://jsiek.github.io/deduce/doc/Reference.html#rewrite-in-proof)
* [`definition`-`in`](https://jsiek.github.io/deduce/doc/Reference.html#definition-in-proof)

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
* Sets (`lib/Set.pf`)
```{.deduce^#member_singleton}
theorem member_singleton: all x:Nat. x ∈ single(5) ∪ single(x)
proof
  arbitrary x:Nat
  evaluate
end
```



<!--
```{.deduce^file=DeduceIntroProof.pf}
import Nat
import DeduceProgramming1
import Set

<<len_empty>>
<<len_one>>
<<len_42>>
<<algebra_example>>
<<modus_ponens_example>>
<<assume_example>>
<<member_singleton>>
```
-->

