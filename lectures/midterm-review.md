# Lecture: Midterm Review

Topics

* Labs and Projects
    * Array Search
    * Flood It: time complexity, Java collection classes
	* Linked Lists with Deduce
    * Proof Exercises with Deduce
    * Quick Reverse with Deduce
    * Mergesort with Iterators 
    * Segment Intersection: BST and AVL
* Arrays, array algorithms e.g. search, rotate, Java's ArrayList
* Half-open ranges
* Testing
* Math: log, exponent
* Logic, Proof, and Deduce
* Linked lists, Functions, Proofs in Deduce
* Abstract data types
    * interfaces
    * iterators
* Time complexity: big-O, asymptotic less-than
* Sorting
  - Insertion Sort
  - Merge Sort
  - Quick Sort
* Binary Search Trees
* Balanced Binary Search Trees via AVL Trees

## Big-O

Def. f is asymptotically less-or-equal to g, written `f ≲ g`,
iff `exists c k. for all n ≥ k. f(n) ≤ c g(n).`

Def. `O(g) = { f | f ≲ g }`

Simplest example

        f(n) = n

        g(n) = n²

        n ≲ n²? aka. n ∈ O(n²)?

        choose k = 0, c = 1

        for all n ≥ 0, n ≤ n² 

Example that requires a more interesting choice of k.

        show that n + 100 ≲ n²
                  n + 100 ∈ O(n²)

        f(n) = n + 100
        g(n) = n²
        goal: exists c k. for all n ≥ k. n + 100 ≤ c * n²
        
        choose c = 1
        goal: exists k. for all n ≥ k. n + 100 ≤ 1 * n²
        choose k = 11                
        goal: for all n ≥ 11. n + 100 ≤ n²
           yeah, that's true, look at the table
           
        n   | n + 100 | n²
        --- | ------- | ----
        1   |  101    | 1
        2   |  102    | 4
		3   |  103    | 9
		... |  ...    | ...
        10  |  110    | 100
        11  |  111    | 121    <---
		12  |  112    | 144

        choose k = 11, c = 1
        for all n ≥ 11, n + 100 ≤ n²

Example that requires a more interesting choice of c.

        3 * n ≲ n

        f(n) = 3 n

        g(n) = n

        if we try c=1, no choice of k will work.

        choose c = 3, then any reasonable k will work
            3 n <= 3 n
            
            3 n <= c n
		    3 n / n <= c n / n 
			3 <= c

        for all n ≥ 0, 3n ≤ 3n.



        5n^2 + 3n + 10 in   O(n^2)
		   choose c=6

Exponents and logarithm

(2 * x) / 2 = x

log(2^x) = x

log(2^0) = log(1) = 0   (n = 1)
log(2^1) = log(2) = 1   (n = 2)
log(2^2) = log(4) = 2   (n = 4)
log(2^3) = 3   (n = 8)
log(2^4) = 4   (n = 16)


Ex. 3.1-1

prove that max(f(n),g(n)) ∈ O(f(n) + g(n))

where

    max(f(n),g(n))(n) = f(n) if f(n) ≥ g(n)
                        g(n) if g(n) > f(n)
Recall:

   f ∈ O(g) iff exists c k st. for all n >= k. f(n) <= c * g(n)

   nts. max(f(n),g(n)) ≤ c * (f(n) + g(n)) for all n >= k

   choose k = 0

   We have f(n) ≤ f(n) + g(n)
   and    g(n) ≤ f(n) + g(n)
   so     max(f(n), g(n)) ≤ f(n) + g(n)
   We choose c to be 1.



## Binary Trees

traversals: 

* pre-order: current, left, right
* in-order: left, current, right
* post-order; left, right, current

                     _____ 25____
                    /            \
                 __15__           50
                /      \         /
               10       22      35
              /  \     /       / 
             4    12  16      31 

pre order traversal: 25, 15, 10, 4, 12, 22, 16, 50, 35, 31
in order traversal:  4, 10, 12, 15, 16, 22, 25, 31, 35, 50
post order traversal: 4, 12, 10, 16, 22, 15, 31, 35, 50, 25

pre-order breadth first:
      25, 15, 50, 10, 22, 35, 4, 12, 16, 31


first/last methods

next/previous methods

    if right child, return right's first
    else find ancestor that comes next wrt. inorder


## Maximum of a Sequence using Iterators

Implement a `max` algorithm that returns the maximum element of a
sequence `s` or return `zero` if none of the elements are larger than
`zero`.

    public static <T> T max(Sequence<T> s, T zero, BiPredicate<T, T> less);

Here are the interfaces:

    interface Sequence<T> {
        Iter<T> begin();
        Iter<T> end();
    }

    interface Iter<T> {
        T get();
        void advance();
        boolean equals(Iter<T> other);
        Iter<T> clone();
    }

    interface BiPredicate {
	    boolean test(T t, U u);
	}

    public static <T> T max(Sequence<T> s, T zero, BiPredicate<T, T> less) {
	   T bestyet = zero;
	   for (iter = s.begin(); ! iter.equals(s.end()); iter.advance()) {
	      if (less.test(bestyet, iter.get()))
		     bestyet = iter.get();
	   }
	   return bestyet;
    }


## Logic and Deduce

What are the options for proving an `all` formula?

What are the options for using an `all` formula?

Ditto for a `some` formula (aka. exists)?

Ditto for an `if`-`then` formula (implication)?

Ditto for an `and` formula (conjunction)?

Ditto for an `or` formula (conjunction)?

## Linked Lists

### Sum-Append Proof

The sum of a list is defined as follows

    sum(empty) = 0
    sum(node(x, xs)) = x + sum(xs)

Recall that operator ++ appends two lists together:

    empty ++ ys = ys
    node(n, xs) ++ ys = node(n, xs ++ ys)

Prove that

    sum(xs ++ ys) = sum(xs) + sum(ys)

	
