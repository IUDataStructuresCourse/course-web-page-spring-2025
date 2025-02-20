# Binary Search Trees (BST)

Idea: use binary trees to implement the Set interface, such that doing
a search for an element is like doing binary search.

```java
interface Set<T> {
   void insert(T e);
   void remove();
   boolean member(T e); // aka contains
   ...
}
```

The **Binary-Search-Tree Property**:
For every node x in the tree,
1. if node y is in the left subtree of x, then `y.data < x.data`, and
2. if node z is in the right subtree of x, then `x.data < z.data`.

We can also use BSTs to implement the Map interface (aka. "dictionary").

```java
interface Map<K,V> {
   V get(K key);
   V put(K key, V value);
   V remove(K key);
   boolean containsKey(K key);
}
```

## Binary Search Tree and Node Classes

`BinarySearchTree` is like `BinaryTree` (has a `root`) but also
has a `lessThan` predicate for comparing elements.

``` java
class Node<K> {
    K data;
    Node<K> left, right;
    // ...
}

class BinarySearchTree<K> implements Set<K> {
    Node<K> root;
    protected BiPredicate<K, K> lessThan;
	BinarySearchTree(BiPredicate<K, K> less) { lessThan = less; }
    // ...
}
```

## `find`, `search`, and `contains` methods of `BinarySearchTree`

Book 4.3.1.

Example: Search for 6, 9, 15 in the following tree:

```
          8
        /   \
       /     \
      3       10
     / \        \
    1   6       14
       / \     /
      4   7   13
```

The `find()` method looks for the node with the specified key; if there is none,
it returns the parent of where such a node would be.

```java
protected Node<K> find(K key, Node<K> curr, Node<K> parent) {
	if (curr == null)
		return parent;
	else if (lessThan.test(key, curr.data))
		return find(key, curr.left, curr);
	else if (lessThan.test(curr.data, key))
		return find(key, curr.right, curr);
	else
		return curr;
}
```

The `search()` method looks for the specified key and returns the node
if the key is found and otherwise returns `null`.

```java
public Node<K> search(K key) {
	Node<K> n = find(key, root, null);
	if (n == null)
	  return null;
	else if (n.data.equals(key)))
	  return n;
	else
	  return null;
}
```

The `contains()` method returns true if the key is found and false otherwise.

```java
public boolean contains(K key) {
	Node<K> p = search(key);
	return p != null;
}
```

What is the time complexity? O(h), where h is the height of the tree.

## `insert` method of `BinarySearchTree`

Textbook 4.3.3.

Similarly we can perform insertions on BSTs. The `insert()` method
adds the given key to the tree.

```java
public void insert(K key) {
	root = insert_helper(key, root);
}

protected static Node<K> insert_helper(K key, Node<K> curr) {
	if (curr == null)
		return new Node<>(key, null, null);
	else if (lessThan.test(key, curr.data)) {
		curr.left = insert_helper(key, curr.left);
        return curr;
	} else if (lessThan.test(curr.data, key)) {
		curr.right = insert_helper(key, curr.right);
        return curr;
	} else {
		// duplicate; do nothing
        return curr;
	}
}
```

(There is an alternative implementation of `insert` that uses `find`.)

What is the time complexity? O(h), where h is the height of the tree.


## `remove`  method of `BinarySearchTree`

Book 4.3.4.

* Case 1: no left child: grandparent adopts the child

```
          o              o
          |              |
        z=o              A
           \       ==>
            A
```

* Case 2: no right child: grandparent adopts the child

```
            o            o
            |            |
          z=o            A
           /       ==>
          A
```

* Case 3: two children

```
             |
           z=o
            / \
           A   B
```

The main idea is to replace z with the node after z, which is the
first node y in subtree B wrt. inorder traveral. We then recursively
delete y from B.

Here is the `first` method of the `Node` class. The idea is to keep going
left until you find a leaf node.

```
Node<K> first() {
    if (this.left == null)
        return this;
    return this.left.first();
}
```


## Example of `remove`

Remove node 8 from the following tree

```
             8
           /   \
          5     10
         / \   / \
        2   6 9   11
       / \   \      \
      1   3   7      12
           \
            4
```

8 has two children, so we replace 8 with the first node in the subtree of 10,
which is node 9.

```
             9
           /   \
          5     10
         / \     \
        2   6     11
       / \   \      \
      1   3   7      12
           \
            4
```

## Implementation of `remove`

Implementation of `remove` (similar to book Figure 4.25):

```java
public void remove(K key) {
	root = remove_helper(root, key);
}

private Node remove_helper(Node<K> curr, K key) {
	if (curr == null) {
		return null;
	} else if (lessThan.test(key, curr.data)) { // remove in left subtree
		curr.left = remove_helper(curr.left, key);
		return curr;
	} else if (lessThan.test(curr.data, key)) { // remove in right subtree
		curr.right = remove_helper(curr.right, key);
		return curr;
	} else {      // remove this node
		if (curr.left == null) {
			return curr.right;
		} else if (curr.right == null) {
			return curr.left;
		} else {   // two children, replace with first of right subtree
			Node<K> min = curr.right.first();
			curr.data = min.data;
			curr.right = remove_helper(curr.right, min.data);
			return curr;
		}
	}
}
```

## Time complexity of `remove`

What is the time complexity? 
Let h be the height of the tree.
The time complexity is O(h).


