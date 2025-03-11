# Binary Trees

## Motivation 

Taking stock with respect to Flood-it! and the Set interface

|                  |  array | linked list | sorted array |
| ---------------- |:------:|:-----------:|:------------:|
| membership test  | O(n)   | O(n)        | O(log(n))    |
| insertion        | O(1)*  | O(1)        | O(n)         |

* How do we keep the array sorted when inserting more elements?

	Use binary search for insertion.

	What's the time complexity?

	O(n) because we still have to shift elements down.

* We'd like something that can do both membership test and insertion in
  O(lg n).

* Can we create a hybrid of the sorted array and the linked list?

	Yes: binary search trees!

* This lecture we discuss binary trees as a lead-up to discussing
  binary search trees in the next lecture.

## BinaryNode and BinaryTree Classes

	class BinaryNode<T> {
		T data;
		BinaryNode<T> left;
		BinaryNode<T> right;

		BinaryNode(T d, BinaryNode<T> l, BinaryNode<T> r) {
				data = d; left = l; right = r;
		}
	}

	public class BinaryTree<T> {
		BinaryNode<T> root;

		public BinaryTree() { root = null; }
		// ...
	}

We can traverse a binary tree in several different ways:
* preorder: me, left, right
* inorder: left, me, right
* postorder: left, right, me

```
preorder(node) {
   output node.data
   preorder(node.left)
   preorder(node.right)
}

inorder(node) {
   inorder(node.left)
   output node.data
   inorder(node.right)
}

postorder(node) {
   postorder(node.left)
   postorder(node.right)
   output node.data
}
```

Example:

                 10
                / \
               /   \
              5     14
             / \     \
            2   7     19

            pre:  10,5,2,7,14,19
            in:   2,5,7,10,14,19
            post: 2,7,5,19,14,10

Recursion Tree written as an "outline"
* pre(Node(10))
  * me: output 10
  * left: pre(Node(5)
    * me: output 5
	* left: pre(Node(2))
	  * me: output 2
	  * left: null
	  * right: null
	* right: pre(Node(7))
	  * me: output 7
	  * left: null
	  * right: null
  * right: pre(Node(14)
    * me: output 14
	* left: null
	* right: pre(Node(19))
	  * me: output 19
	  * left: null
	  * right: null

