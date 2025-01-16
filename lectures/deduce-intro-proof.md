# Writing Proofs in Deduce

To get started, we write down the theorem we would like to prove.  We
start with the `theorem` keyword followed by a label, a colon, and the
formula. The proof of the theorem starts with the keyword `proof`,
then the actual proof, and ends with the keyword `end`.  In the
following, instead of writing the proof, we'll simply write `?` to say
that we're not done yet.

```
theorem len_empty: len(Empty) = 0
proof
  ?
end
```

Run Deduce on the file. Deduce will respond with the following message
to remind us of what is left to prove.

```
incomplete proof
Goal:
	len(Empty) = 0
```

Recall that the definition of the `len` function says that
`len(Empty) = 0`, so we simply need to tell Deduce to
use the definition of `len`, which we can do using
the [`definition`](https://jsiek.github.io/deduce/doc/Reference.html#definition-proof-)
proof statement.

```{.deduce^#len_empty}
theorem len_empty: len(Empty) = 0
proof
  ? //definition len
end
```


<!--
```{.deduce^file=DeduceIntroProof.pf}
import Nat
import DeduceProgramming1

<<len_empty>>

```
-->
