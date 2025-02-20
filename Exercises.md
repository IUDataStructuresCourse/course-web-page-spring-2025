# More Deduce Proof Exercises

## Mirror Mirror on the Wall

Prove that two applications of the below `mirror` function to a binary
tree results in the original binary tree.

```
union Tree {
  EmptyTree
  TreeNode(Tree, Nat, Tree)
}

recursive mirror(Tree) -> Tree {
  mirror(EmptyTree) = EmptyTree
  mirror(TreeNode(L, x, R)) =
    TreeNode(mirror(R), x, mirror(L))
}

theorem mirror_mirror: all T:Tree. mirror(mirror(T)) = T
proof
  ?
end
```

## Flat Mirrors

Prove that mirroring and then flattening a tree is the same as
flattening and then reversing the tree.

```
recursive flatten(Tree) -> List<Nat> {
  flatten(EmptyTree) = []
  flatten(TreeNode(L, x, R)) =
    flatten(L) ++ [x] ++ flatten(R)
}

theorem flat_mirror: all T:Tree. flatten(mirror(T)) = reverse(flatten(T))
proof
  ?
end
```
