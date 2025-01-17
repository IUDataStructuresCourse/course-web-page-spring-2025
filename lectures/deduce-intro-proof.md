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
* [suffices](https://jsiek.github.io/deduce/doc/Reference.html#suffices-proof-statement), and
* [evaluate](https://jsiek.github.io/deduce/doc/Reference.html#evaluate).

```{.deduce^#len_one}
theorem len_one: all x:Nat. len(Node(x, Empty)) = 1
proof
  arbitrary x:Nat
  suffices 1 + 0 = 1 by definition {len, len}
  evaluate
end
```

* [proof instantiation](https://jsiek.github.io/deduce/doc/Reference.html#instantiation-proof)
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
