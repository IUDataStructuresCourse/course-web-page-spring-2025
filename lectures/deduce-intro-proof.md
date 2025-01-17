# Writing Proofs in Deduce

* [`theorem`](https://jsiek.github.io/deduce/doc/Reference.html#theorem-statement)
* [`definition`](https://jsiek.github.io/deduce/doc/Reference.html#definition-proof)

```{.deduce^#len_empty}
theorem len_empty: 0 = len(Empty)
proof
  definition len
end
```

* [`all`](https://jsiek.github.io/deduce/doc/Reference.html#all-universal-quantifier) formulas,
* [`arbitrary`](https://jsiek.github.io/deduce/doc/Reference.html#arbitrary-forall-introduction) statements,
* [proof instantiation](https://jsiek.github.io/deduce/doc/Reference.html#instantiation-proof),
* [suffices](https://jsiek.github.io/deduce/doc/Reference.html#suffices-proof-statement), and
* [evaluate](https://jsiek.github.io/deduce/doc/Reference.html#evaluate).

```{.deduce^#len_one}
theorem len_one: all x:Nat. 1 = len(Node(x, Empty))
proof
  arbitrary x:Nat
  suffices 1 = 1 + 0 by definition {len, len}
  evaluate
end
```

<!--
```{.deduce^file=DeduceIntroProof.pf}
import Nat
import DeduceProgramming1

<<len_empty>>
<<len_one>>

```
-->
