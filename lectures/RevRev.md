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
  case [] {
    conclude reverse(reverse(@[]<U>)) = [] by definition reverse
  }
  case node(n, ls') suppose IH: reverse(reverse(ls')) = ls' {
    equations
          reverse(reverse(node(n,ls')))
        = reverse(reverse(ls') ++ [n])         by definition reverse
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
        = reverse(reverse(ls') ++ [n])         by definition reverse
    ... = [n] ++ reverse(reverse(ls'))         by ?
    ... = [n] ++ ls'                           by IH
    ... = node(n,ls')                          by definition operator++
```

So would could try to prove the following lemma.
```
lemma first_lemma: all U:type, ls:List<U>, n:U.
  reverse(reverse(ls) ++ [n]) = [n] ++ reverse(reverse(ls))
```

However, this lemma is overly complicated because it includes two uses
of `reverse` on the left-hand side, but the inner one is just along
for the ride.  So we can generalize the lemma a little bit by
replacing `reverse(ls')` with `ls'`.

```{.deduce^#second_lemma}
lemma second_lemma: all U:type, ls':List<U>, n:U.
  reverse(ls' ++ [n]) = [n] ++ reverse(ls')
proof
  arbitrary U:type
  induction List<U>
  case [] {
    arbitrary n:U
    conclude reverse(@[]<U> ++ [n]) = [n] ++ reverse(@[]<U>)
      by evaluate
  }
  case node(x, ls') 
    assume IH: (all n:U. reverse(ls' ++ [n]) = [n] ++ reverse(ls')) {
    arbitrary n:U
    equations
          reverse(node(x, ls') ++ [n]) 
        = reverse(node(x, ls' ++ [n]))     by definition operator++
    ... = reverse(ls' ++ [n]) ++ [x]       by definition reverse
    ... = ([n] ++ reverse(ls')) ++ [x]     by rewrite IH[n]
    ... = [n] ++ reverse(ls') ++ [x]       by append_assoc<U>
    ... = [n] ++ #reverse(node(x, ls'))#   by definition reverse
  }
end
```

The above `second_lemma` gets the job done, but it is slightly
inelegant because the `[n]` is not as general as it could be.  We can
replace `[n]` with an arbitrary list `ys`. However, when we do so, we
need to reverse it on the other side of the equality. Now we have a
beautiful lemma that says if you append two lists then reverse the
result, that's the same as reversing the two lists and then appending
the two results.

```{.deduce^#reverse_append}
lemma rev_append: all U :type. all xs :List<U>. all ys :List<U>.
  reverse(xs ++ ys) = reverse(ys) ++ reverse(xs)
proof
  arbitrary U :type
  induction List<U>
  case [] {
    arbitrary ys :List<U>
    equations
          reverse(@[]<U> ++ ys)
        = reverse(ys)                     by definition operator++
    ... = # reverse(ys) ++ [] #           by rewrite append_empty<U>[reverse(ys)]
    ... = # reverse(ys) ++ reverse([]) #  by definition reverse
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

With the `rev_append` lemma in hand, we can complete the
proof of the `reverse_reverse` theorem.

```{.deduce^#reverse_reverse}
theorem reverse_reverse: all U :type. all ls :List<U>.
  reverse(reverse(ls)) = ls
proof
  arbitrary U :type
  induction List<U>
  case [] {
    conclude reverse(reverse(@[]<U>)) = []   by definition reverse
  }
  case node(n, ls') suppose IH: reverse(reverse(ls')) = ls' {
    equations
      reverse(reverse(node(n,ls')))
          = reverse(reverse(ls') ++ [n])           by definition reverse
      ... = reverse([n]) ++ reverse(reverse(ls'))  by rev_append<U>[reverse(ls')][[n]]
      ... = reverse([n]) ++ ls'                    by rewrite IH
      ... = node(n,ls')                            by definition {reverse,reverse,operator++}
  }
end
```


<!--
```{.deduce^file=Revrev.pf}
import List
import Nat

<<reverse_reverse_first_attempt>>
<<second_lemma>>
<<reverse_append>>
<<reverse_reverse>>
```
-->
