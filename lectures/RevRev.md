# Discovering and Generalizing Lemmas

When theorems get more complicated, we sometimes need to prove
auxilliary theorems (aka. lemmas) to get past sticky parts of the
proof. Formulating these lemmas takes some skill, so it is worth
seeing an example of how to do that.

Let's try to prove a simple property of `reverse`. Applying
`reverse` twice should take us back to the original list.

```
reverse(reverse(ls)) = ls
```

We dive into the proof, doing induction on the list. However, we get
stuck because we can't apply the induction hypothesis. There's an
append in between the two reverses.

```{.deduce^#reverse_reverse_first_attempt}
theorem reverse_reverse_first_attempt: all U :type. all ls :List<U>.
  reverse(reverse(ls)) = ls
proof
  arbitrary U :type
  induction List<U>
  case empty {
    conclude reverse(reverse(@empty<U>)) = empty by definition reverse
  }
  case node(n, ls') suppose IH: reverse(reverse(ls')) = ls' {
    equations
          reverse(reverse(node(n,ls')))
        = reverse(reverse(ls') ++ [n])         by definition {reverse,operator++}
    ... = node(n,ls')                          by sorry
  }
end
```

The following equation ought to be true:

```
reverse(reverse(ls') ++ [n]) = [n] ++ reverse(reverse(ls'))
```

and if it were, then we could complete the proof as follows

```
    equations
          reverse(reverse(node(n,ls')))
        = reverse(reverse(ls') ++ [n])         by definition {reverse,operator++}
    ... = [n] ++ reverse(reverse(ls'))         by ?
    ... = [n] ++ ls'                           by IH
    ... = node(n,ls')                          by definition {operator++}
```

So would could try to prove the following lemma.
```
lemma first_lemma: all U:type. all ls':List<U>, all n:U.
  reverse(reverse(ls') ++ [n]) = [n] ++ reverse(reverse(ls'))
```
However, proving this lemma would be difficult or impossible. The first
problem is that this lemma is overly complicated because it include two uses of
`reverse` on the left-hand side, but the inner one is just along for
the ride.  So we can generalize the lemma a little bit but replacing
`reverse(ls')` with `ls'`.

```
lemma second_lemma: all U:type. all ls':List<U>, all n:U.
  reverse(ls' ++ [n]) = [n] ++ reverse(ls')
```

The next problem is that the `[n]` is overly specific, which will give
us problems with the induction hypothesis. Again, we can generalize,
this time replacing `[n]` with a new list variable. This brings us to
the following formulation of the lemma.

```{.deduce^#reverse_append}
lemma reverse_append: all U :type. all xs :List<U>. all ys :List<U>.
  reverse(xs ++ ys) = reverse(ys) ++ reverse(xs)
proof
  arbitrary U :type
  induction List<U>
  case empty {
    arbitrary ys :List<U>
    equations
          reverse(@empty<U> ++ ys)
        = reverse(ys)                   by definition operator++
    ... = # reverse(ys) ++ empty #       by rewrite append_empty<U>[reverse(ys)]
    ... = # reverse(ys) ++ reverse(empty) # by definition reverse
  }
  case node(n, xs') suppose IH {
    arbitrary ys :List<U>
    equations
          reverse(node(n,xs') ++ ys)
        = reverse(node(n, xs' ++ ys))                by definition operator++
    ... = reverse(xs' ++ ys) ++ [n]                  by definition reverse
    ... = (reverse(ys) ++ reverse(xs')) ++ [n]       by rewrite IH[ys]
    ... = reverse(ys) ++ (reverse(xs') ++ [n])       by append_assoc<U>[reverse(ys)][reverse(xs'), [n]]
    ... = # reverse(ys) ++ reverse(node(n,xs')) #    by definition reverse
  }
end
```

With the `reverse_append` lemma is hand, it is now straightforward to
prove the `reverse_reverse` theorem.

```{.deduce^#reverse_reverse}
theorem reverse_reverse: all U :type. all ls :List<U>.
  reverse(reverse(ls)) = ls
proof
  arbitrary U :type
  induction List<U>
  case empty {
    conclude reverse(reverse(@empty<U>)) = empty by definition reverse
  }
  case node(n, ls') suppose IH: reverse(reverse(ls')) = ls' {
    equations
      reverse(reverse(node(n,ls')))
          = reverse(reverse(ls') ++ [n])           by definition {reverse,operator++}
      ... = reverse([n]) ++ reverse(reverse(ls'))  by reverse_append<U>[reverse(ls')][[n]]
      ... = reverse([n]) ++ ls'                    by rewrite IH
      ... = node(n,ls')                            by definition {reverse,reverse,operator++, operator++}
  }
end
```


<!--
```{.deduce^file=Revrev.pf}
import List
import Nat

<<reverse_reverse_first_attempt>>
<<reverse_append>>
<<reverse_reverse>>
```
-->
