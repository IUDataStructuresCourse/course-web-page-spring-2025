# Proof: Quick Reverse Correct

Prove that the `quick_rev` function that you implemented in lab
is equivalent to the `reverse` function in `lib/List.pf`.

```
theorem quick_rev_correct: <T> all xs : List<T>.
  quick_rev(xs) = reverse(xs)
proof
  ?
end
```

The file you submit to the autograder must be named `QuickRevCorrect.pf`
and it should include the above theorem and the definition of `quick_rev`
and any helper functions that you created to use in `quick_rev`.
Your `QuickRevCorrect.pf` should only import files from the Deduce
`lib` directory.
