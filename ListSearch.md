# Linear Search in a Linked List (Variant)

Prove the following theorem about the variant of `search` that you
implemented in lab last week.

```
theorem search_correct: all y: Nat. all xs: List<Nat>.
    define front = first(search(xs, y));
    define back = second(search(xs, y));
    (back = [] or head(back) = just(y)) and
    not (y ∈ set_of(front)) and
    front ++ back = xs 
proof
  ?
end
```



