## Review: AVL Invariant

Recall the definition of the AVL invariant

**Definition** The AVL Invariant: the height of two child subtrees may
only differ by 1, for every node in the tree.

## Tree Rotation

                    y                         x
                   / \    right_rotate(y)    / \
                  x   C  --------------->   A   y
                 / \     <-------------        / \
                A   B     left_rotate(x)      B   C

Rotations preserve the BST property and the in-order ordering.

    A x B y C  =  A x B y C


## Algorithm for fixing AVL property

Starting from the lowest changed node, repeat the following up to the root of
the tree (because there can be several AVL violations).
* check whether node x is AVL, if not do the following.
* if x.left.height < x.right.height

    1. if x.right.left.height ≤ x.right.right.height

        let k = x.right.right.height

                    x k+2                                y k+1
                   / \                L(x)              / \
              k-1 A   y k+1     ===============>     k x   C k
                     / \                              / \
                k-1 B   C k                      k-1 A   B k-1

    2. if x.right.left.height > x.right.right.height

        let k = x.right.left.height

              k+2 x                               y k+1
                 / \                            /   \
            k-1 A   z k+1    R(z), L(x)      k x     z k
                   / \      =============>    / \   / \
                k y   D k-1              k-1 A   B C   D k-1
                 / \
                B   C <k

* if height(x.left) > height(x.right)

    1. if height(x.left.left) < height(x.left.right)  (note: strictly less!)

        let k = height(x.left.right)

                    x k+2                                 z k+1
                   / \                                  /   \
              k+1 y   D k-1      L(y), R(x)          k y     x k
                 / \            =============>        / \   / \
            k-1 A   z k                              A   B C   D    <k
                   / \
                  B   C <k

    2. if height(x.left.left) ≥ height(x.left.right)  (note: greater-equal!)

        let k = height(l.left.left)

                  x k+2                                  y k+1
                 / \               R(x)                 / \
            k+1 y   C k-1    ===============>        k A   x k+1
               / \                                        / \
            k A   B ≤k                                ≤k B   C k-1


## Time Complexity of Insert and Remove for AVL Tree

1. Add or remove using the BST algorithm: O(log n)

2. Number of iterations of the fixing AVL: O(log n)
   How much time per fix: O(1)
   O(log n) * O(1) = O(log n)

Taotal for add/remove and rebalance: O(log n) +  O(log n) = O(log n)


## Using AVL Trees for sorting

1. Insert n items: O(n log(n))

2. In-order traversal: O(n)

Overall time complexity: O(n log(n))


## ADT's that can be implemented by AVL Trees

* priority queue:
  insert, delete, min
  
* ordered set:
  insert, delete, min, max, next, previous
  

## Does the AVL invariant ensure that the tree is balanced?

Let N(h) represent the minimum number of nodes in an AVL tree of
height h. (The least-balanced possible scenario.)

    N(h) = N(h-1) + N(h-2) + 1

We want to show that

    h ≲ log₂(N(h))

Ok, let's use the equation for `N(h)` and see if we can relate it to `h`.

    N(h) = N(h-1) + N(h-2) + 1
         > N(h-2) + N(h-2) + 1
         = 2·N(h-2) + 1
         > 2·N(h-2)
         > 2·2·N(h-4)       because ∀ h. N(h) > 2·N(h-2)
         > 2·2·2·N(h-6)
           ...
         > 2^(h/2)

so

    N(h) > 2^(h/2)
    
Take the log of both sides

          log₂(N(h)) > log₂(2^(h/2))
                                  (log₂(Aᴮ) = B log₂(A))
                     = h/2 · log₂(2)
                                  (log₂(2) = 1, because 2¹ = 2)
                     = h/2 · 1
                                  (multiply both side by 2) 
    2 · log₂(N(h)) > h

so we have

    h ≲ log₂ N(h)

