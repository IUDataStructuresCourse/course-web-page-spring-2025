# Writing Proofs in Deduce

Concepts:
* [`theorem`](https://jsiek.github.io/deduce/pages/reference.html#theorem-statement) statement
* [`definition`](https://jsiek.github.io/deduce/pages/reference.html#definition-proof) proof

Example:
```{.deduce^#len_empty}
theorem len_empty: 0 = len(Empty)
proof
  definition len
end
```

Concepts:
* [`all`](https://jsiek.github.io/deduce/pages/reference.html#all-universal-quantifier) formula,
* [`arbitrary`](https://jsiek.github.io/deduce/pages/reference.html#arbitrary-forall-introduction) proof,
* [`suffices`](https://jsiek.github.io/deduce/pages/reference.html#suffices-proof-statement) proof, and
* [`evaluate`](https://jsiek.github.io/deduce/pages/reference.html#evaluate-proof) proof.

Example:
```{.deduce^#len_one}
theorem len_one: all x:Nat. len(Node(x, Empty)) = 1
proof
  arbitrary x:Nat
  suffices 1 + 0 = 1 by definition {len, len}
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
```
-->

