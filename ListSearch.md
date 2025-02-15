# Lab: List Search

Prove the following theorem about the `search` provided in the
following file that you should `import`.

[Search.pf](./Search.pf)


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



