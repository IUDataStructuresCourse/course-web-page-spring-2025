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
* [suffices](https://jsiek.github.io/deduce/doc/Reference.html#suffices-proof-statement) proof, and
* [evaluate](https://jsiek.github.io/deduce/doc/Reference.html#evaluate) proof.

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
* [rewrite](https://jsiek.github.io/deduce/doc/Reference.html#rewrite-proof)

Example:
```{.deduce^#len_42}
theorem len_42:  1 = len(Node(42, Empty))
proof
  rewrite len_one[42]
end
```

<!--
```{.deduce^file=DeduceIntroProof.pf}
import Nat
import DeduceProgramming1

<<len_empty>>
<<len_one>>
<<len_42>>
```
-->
