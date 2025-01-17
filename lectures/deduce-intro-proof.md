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
* [If Then](https://jsiek.github.io/deduce/doc/Reference.html#if-then-conditional-formula) formula
* [`have`](https://jsiek.github.io/deduce/doc/Reference.html#have-proof-statement) proof
* [apply-to](https://jsiek.github.io/deduce/doc/Reference.html#apply-to-proof-modus-ponens) proof

From `lib/Nat.thm`:
```
max_equal_greater_right: (all x:Nat. (all y:Nat. (if x ≤ y then max(x, y) = y)))
```

```{.deduce^#modus_ponens_example}
theorem modus_ponens_example: all x:Nat. max(x, 1 + x) = 1 + x
proof
  arbitrary x:Nat
  have: x ≤ 1 + x   by less_equal_add_left
  conclude max(x, 1 + x) = 1 + x  by apply max_equal_greater_right to recall x ≤ 1 + x
end
```





Example:
```{.deduce^#have_example}
TBD
```




<!--
```{.deduce^file=DeduceIntroProof.pf}
import Nat
import DeduceProgramming1

<<len_empty>>
<<len_one>>
<<len_42>>
<<algebra_example>>
<<modus_ponens_example>>
```
-->

