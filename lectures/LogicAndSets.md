## Reasoning about Logical And

Concepts:
* [`and`](https://jsiek.github.io/deduce/pages/reference.html#and-logical-conjunction) formula
* [comma](https://jsiek.github.io/deduce/pages/reference.html#comma-logical-and-introduction) proof,

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
      conclude 0 = 0 and m = 0 by . , recall m = 0     // <-- Here is the comma!
    }
    case suc(n') {
      suppose premise: suc(n') + m = 0
      have eq: 1 + (n' + m) = 0 
          by rewrite suc_one_add | add_assoc in premise
      conclude false by apply not_one_add_zero to eq
    }
  }
end

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
  conclude length(xs) = 0 by both            // <-- Implicit use of one conjunct
end
```

## Reasoning about Logical Or

Concepts:
* [`or`](https://jsiek.github.io/deduce/pages/reference.html#or-logical-disjunction) formula
* [`cases`](https://jsiek.github.io/deduce/pages/reference.html#cases-disjunction-elimination) proof

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

## Reasoning about Logical Not

Concepts:
* [`not`](https://jsiek.github.io/deduce/pages/reference.html#not) formula

```{.deduce^#not_example}
theorem not_example: not (0 = 1)
proof
  assume: 0 = 1
  conclude false by recall 0 = 1
end
```


## Reasoning about Sets

Concepts:
* Set library (`lib/Set.pf`)
* The type of a mathematical set: `Set<T>`
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


Concepts:
* There are many theorems about sets in `lib/Set.pf`, such as
  `empty_union` and `union_assoc`.

Example:
```{.deduce^#setof_append}
theorem setof_append: <T> all xs:List<T>, ys:List<T>.
  set_of(xs ++ ys) = set_of(xs) ∪ set_of(ys)
proof
  arbitrary T:type
  induction List<T>
  case [] {
    arbitrary ys:List<T>
    suffices set_of(ys) = ∅ ∪ set_of(ys)   by definition {operator++, set_of}
    rewrite empty_union<T>
  }
  case node(x, xs') assume IH: all ys:List<T>. set_of(xs' ++ ys) = set_of(xs') ∪ set_of(ys) {
    arbitrary ys:List<T>
    equations
          set_of(node(x, xs') ++ ys) 
        = single(x) ∪ set_of(xs' ++ ys)           by definition {operator++, set_of}
    ... = single(x) ∪ (set_of(xs') ∪ set_of(ys))  by rewrite IH
    ... = (single(x) ∪ set_of(xs')) ∪ set_of(ys)  by symmetric union_assoc<T>
    ... = #set_of(node(x, xs'))# ∪ set_of(ys)     by definition set_of
  }
end
```


<!--
```{.deduce^file=LogicAndSets.pf}
import Nat
import List
import Set

<<add_to_zero_is_zero>>
<<intro_dichotomy>>
<<not_example>>
<<member_set_of>>
<<member_singleton>>
<<member_union>>
<<member_intersection>>
<<setof_append>>
```
-->
