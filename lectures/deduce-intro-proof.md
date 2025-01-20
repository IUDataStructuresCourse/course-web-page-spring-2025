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
* [`true`](https://jsiek.github.io/deduce/doc/Reference.html#true-formula) formula
* [period](https://jsiek.github.io/deduce/doc/Reference.html#period-proof-of-true)
* [`false`](https://jsiek.github.io/deduce/doc/Reference.html#false) formula

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
* [`and`](https://jsiek.github.io/deduce/doc/Reference.html#and-logical-conjunction) formula
* [comma](https://jsiek.github.io/deduce/doc/Reference.html#comma-logical-and-introduction) proof,

Example:
```{.deduce^#add_to_zero_is_zero}
theorem add_to_zero_is_zero: all n:Nat, m:Nat.
  if n + m = 0
  then n = 0 and m = 0
proof
  arbitrary n:Nat, m:Nat
  switch n {
    case 0 {
      assume premise: 0 + m = 0
      have: m = 0  by definition operator + in premise
      conclude 0 = 0 and m = 0 by . , recall m = 0
    }
    case suc(n') {
      suppose premise: suc(n') + m = 0
      have eq: 1 + (n' + m) = 0 
          by rewrite suc_one_add[n'] | add_assoc[1,n',m] in premise
      conclude false by apply not_one_add_zero to eq
    }
  }
end

import List

theorem example: all T:type, xs:List<T>, ys:List<T>.
  if length(xs ++ ys) = 0
  then length(xs) = 0
proof
  arbitrary T:type, xs:List<T>, ys:List<T>
  assume prem: length(xs ++ ys) = 0
  have eq1: length(xs ++ ys) = length(xs) + length(ys)  by length_append<T>
  have eq2: length(xs) + length(ys) = 0
      by transitive (symmetric eq1) prem
  have both: length(xs) = 0 and length(ys) = 0 
      by apply add_to_zero_is_zero to eq2
  conclude length(xs) = 0 by both
end
```

Concepts:
* [`or`](https://jsiek.github.io/deduce/doc/Reference.html#or--logical-disjunction) formula
* [`cases`](https://jsiek.github.io/deduce/doc/Reference.html#cases-disjunction-elimination) proof

```{.deduce^#intro_dichotomy}
theorem intro_dichotomy:  all x:Nat, y:Nat.  x ≤ y  or  y < x
proof
  arbitrary x:Nat, y:Nat
  have tri: x < y or x = y or y < x by trichotomy[x][y]
  cases tri
  case: x < y {
    have: x ≤ y by apply less_implies_less_equal[x][y] to recall x < y
    conclude x ≤ y or y < x by recall x ≤ y
  }
  case: x = y {
    have: x ≤ y by
        suffices y ≤ y  by rewrite (recall x = y)
        less_equal_refl[y]
    conclude x ≤ y or y < x by recall x ≤ y
  }
  case: y < x {
    conclude x ≤ y or y < x by recall y < x
  }
end
```

Concepts:
* [`not`](https://jsiek.github.io/deduce/doc/Reference.html#not) formula

```{.deduce^#not_example}
theorem not_example: not (0 = 1)
proof
  assume: 0 = 1
  conclude false by recall 0 = 1
end
```

Concepts:
* Sets (`lib/Set.pf`)
* `set_of` to convert a list to a set
* `single(x)` for a set with one element that is `x`
* `∪` for union
* `∩` for intersection

Example:
```{.deduce^#member_set_of}
theorem member_set_of: 4 ∈ set_of([3,4,5])
proof
  evaluate
end
```

Example:
```{.deduce^#member_singleton}
theorem member_singleton: all x:Nat. x ∈ single(x)
proof
  arbitrary x:Nat
  evaluate
end
```

Example:
```{.deduce^#member_union}
theorem member_union_example: 9 ∈ set_of([2,5]) ∪ set_of([5,9])
proof
  evaluate
end
```

Example:
```{.deduce^#member_intersection}
theorem member_intersection_example: 5 ∈ set_of([2,5]) ∩ set_of([5,9])
proof
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
<<prove_true>>
<<false_explosion>>
<<add_to_zero_is_zero>>
<<intro_dichotomy>>
<<not_example>>

<<member_set_of>>
<<member_singleton>>
<<member_union>>
<<member_intersection>>
```
-->

