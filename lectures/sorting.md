# Lecture: Sorting


## Wrap up on proofs of program correctness

Take away points:
* a computer-checked proof of correctness is the only way to guarantee 100% correctness
* proofs of correctness take a lot of time
* testing takes much less time, but no guarantees
* a deeper understand of logic and proofs of correctness gives you a different
  perspective when writing code and tests that can help prevent bugs

Historical and Industry Perspective:
* Proof assistant technology is relatively new (first mature tools in early 2000's)
  and most of the tools are "expert only" tools.
* New technologies take time to transfer into industry and even longer
  to transfer into education.
* Many large companies (Intel, Facebook, Microsoft) have small teams that
  prove correctness, but formal methods are not widely used.

## Sorting

```
[2,8,7,1,3,5,6,4]
=>
[1,2,3,4,5,6,7,8]
```

Of course, the output should be increasing:

```
boolean is_sorted(int[] A, int begin, int end) {
    for (int i = begin; i != end; ++i) {
        if (i + 1 != end) {
            if (A[i] > A[i + 1])
                return false;
        }
    }
    return true;
}
```

So to do property-based testing, we'd write:
```
assertTrue(is_sorted(sort(A, 0, A.length)));
```

Does this output make sense?

```
[2,8,7,1,3,5,6,4]
=>
[1,2,3,5,6,8]
```

Another property: don't drop values







How about this output?

```
[2,8,7,1,3,5,6,4]
=>
[1,2,3,4,5,6,7,8,9,10]
```

Another property: don't add extra values.






Or this output?

```
[2,8,7,1,3,5,6,4]
=>
[1,2,3,3,4,5,6,6,7,8]
```

Another property: don't add duplicates


```
[1,2,3,3,4,5,6,6,7,8]
=>
[1,2,3,4,5,6,7,8]
```

Another property: the number of occurences of each input element should
stay the same in the output.


multi_set(sort(A)) = multi_set(A)
 




What to do with duplicates?

```
[ {Jane, Smith} , { Mike, Summers } , { Susan, Baker }, { John, Smith }, { Linda, Carlisle } ]

=> (sort by last name)

[ { Susan, Baker }, { Linda, Carlisle }, {Jane, Smith} , { John, Smith }, { Mike, Summers }]

=> (also sort by last name)

[ { Susan, Baker }, { Linda, Carlisle }, { John, Smith }, {Jane, Smith}, { Mike, Summers }]
```


A sorting algorithm is *stable* if the order of duplicates in the
output is the same as order of those duplicates in the input with
respect to eachother.


## Insertion Sort

Input is an array:
```
static void insertion_sort(int[] A) {
    for (int j = 1; j != A.length; ++j) { // n iterations, O(n)*n = O(n^2)
        int key = A[j];  // O(1)
        int i = j - 1;  // O(1)
        while (i >= 0 && A[i] > key) { // n iterations, O(1)*n = O(n)
            A[i+1] = A[i];  // O(1)
            i -= 1;// O(1)
        }
        A[i+1] = key;
    }
}
```

Input is a linked list:
```
class Node {
  int data;
  Node next;
}

Node insert(int x, Node L) {
  if (L == null) {
    return new Node(x, null);
  } else {
    if (x <= L.data) {
      return new Node(x, new Node(y, L.next))
    } else {
      return new Node(L.data, insert(x, L.next))
    }
}

Node insertion_sort(Node L) {
  if (L == null) {
    return null;
  } else {
    return insert(L.data, insertion_sort(L.next));
  }
}
```

Time complexity: O(n²)

## Merge Sort

Input is a linked list:
```
static Node merge(Node A, Node B) { //O(n)
    if (A == null) { return B; }
    else if (B == null) { return A; }
    else if (A.data < B.data) {
        return new Node(A.data, merge(A.next, B));
    } else {
        return new Node(B.data, merge(A, B.next));
    }
}

static Node merge_sort(Node N) {
    int length = Utils.length(N);
    if (length <= 1) {
        return N;
    } else {
      int middle = length / 2;
      Node left_side = Utils.take(N, middle);
      Node right_side = Utils.drop(N, middle);
      Node leftSorted = merge_sort(left_side);
      Node rightSorted = merge_sort(right_side);
      return merge(leftSorted, rightSorted);
    }
}
```

Time complexity: O(n log(n))

## Quicksort

Quicksort is worst case O(n²) time. But on average, it takes n log(n).

Quicksort does not use extra memory; it is an inplace algorithm.

Algorithm overview:

1. Choose a **pivot** element and **partition** the array around the pivot,
   that is, each element in the first part is less than the pivot
   and each element in the second part is greater than the pivot.

2. Sort the two parts.

Implementation:

```
void quicksort(int[] A, int begin, int end) {
    if (begin == end) { // base case, empty
      return; // do nothing
    } else {
        int pivot_pos = partition(A, begin, end);
        // left of pivot <= pivot < right of pivot
        quicksort(A, begin, pivot_pos);
        // left side sorted
        quicksort(A, pivot_pos+1, end);
        // right side sorted
        // whole thing sorted? yes, transitivity through the pivot
    }
}
```
## Partition

The `partition` function chooses the pivot and then rearranges the
array around the pivot so that every element less or equal to the
pivot is to the left of the pivot and every element greater than the
pivot is to the right of the pivot.

The choice of pivot is important because it controls the size of the
left and right-hand sides.

An even split of the array is good because it leads to a balanced tree
of recursive procedure calls.

There are many ways to choose the pivot element. One of the simplest
is to choose the last element, but that doesn't guarantee an even
split.

Simple implementation:

    static int partition_ez(int[] A, int begin, int end) {
      ArrayList<Integer> L = new ArrayList<Integer>();
      ArrayList<Integer> R = new ArrayList<Integer>();
      int pivot = A[end - 1];
      for (int i = begin; i != end - 1; ++i) {
         if (A[i] ≤ pivot) { L.add(A[i]); }     // O(1)*
         else { R.add(A[i]);  }
      }
      int pivot_loc = copy(L, A, begin);
      A[pivot_loc] = pivot;
      copy(R, A, pivot_loc + 1);
      return pivot_loc;
    }
    
Time complexity: O(n)
Space complexity: O(n)
    

### Partition in-place

But we can be more space efficient by reusing `A` for `L` and `R`.

Example run of the partition algorithm:

    [2,8,7,1,3,5,6,4]       (draw with extra space between the numbers)

The last element is the pivot:

    [2,8,7,1,3,5,6|4]

We move from left to right, separating the elements into those less
than 4 and those greater than 4.

    [||2,8,7,1,3,5,6|4]
    [2||8,7,1,3,5,6|4]
    [2|8|7,1,3,5,6|4]
    [2|8,7|1,3,5,6|4]

swap 1 with 8 (the front of the sequence of greater-than elements)

    [2,1|7,8|3,5,6|4]

swap 3 with 7

    [2,1,3|8,7|5,6|4]
    [2,1,3|8,7,5|6|4]
    [2,1,3|8,7,5,6||4]

To finish off, we need to put 4 (the pivot) into the middle.
swap 4 with 8

    [2,1,3|4|7,5,6,8]

Looking at a half-way point in the run gives us a good idea
for what the algorithm needs to do.

    [2,1|7,8|3,5,6|4]
         i   j

if A[j] ≤ pivot, swap it with first greater-than elt., increment i and j

    [2,1,3|8,7|5,6|4]
           i   j

if A[j] > pivot, just move on to the next j

    [2,1,3|8,7,5|6|4]
           i     j

Space-efficient implementation:

```
int partition(int[] A, int begin, int end) {
   int pivot = A[end-1];
   int i = begin;
   for (int j = begin; j != end-1; ++j) {
      if (A[j] ≤ pivot) {
         swap(A, i, j);
         ++i;
      }
   }
   swap(A, i, end-1);
   return i;
}
```
### Student exercise

Apply quicksort to the array, write the state of the array after each
step within the partitioning
    
    [6,1,2,3]

quicksort(A, 0, 4)
- partition around 3

    [|6|1,2|3]
    [1|6|2|3]
    [1,2|6|3]
    [1,2|3|6]
     0 1 2 3 4

- quicksort(A, 0, 2)  (doesn't change anything)
  * partition around 2

        [||1|2]
        [1|||2]
        [1|2|]

  * quicksort(A,0,1)
      - partition around 1

          [|||1]
          [|1|]

      - quicksort(A,0,0) returns immediately
      - quicksort(A,2,2) returns immediately
  * quicksort(A,2,2) returns immediately
- quicksort(A, 3, 4)
    * partition around 6

            [|||6]
            [|6|]

    * quicksort(A,3,3) returns immediately
    * quicksort(A,4,4) returns immediately

### Time complexity of partition: Θ(n)

## Worst-case time complexity of quicksort

  Suppose that each time we partition, it turns out
  that everything ends up in L. So the recursive call
  to sort L is T(n-1) and G is T(0).

      T(0) = 1
      T(n) = T(n-1) + T(0) + n
      
      T(n) = T(n-1) + T(o) + n
           = (T(n-2) + T(0) + n-1) + T(o) + n
           = T(n-2) + 2 + n + n-1
           = T(n-3) + 3 + n + n-1 + n-2
      
      T(n) in O(n²)

## Best-case time complexity

Suppose that each time we partition, L and G come out the same size.
    
    T(n) = T(n/2) + T(n/2) + Θ(n) = 2 T(n/2) + Θ(n)
    
    T(n) in O(n log(n))


### Random Pivot

A better way to choose the pivot is to choose an element at random.

* This makes it difficult for a hacker to take advantage
  of quicksort in a denial-of-service attack.

* Also, the probability of the random choice landing at the
  far ends of the array is low, so the splits are even enough
  to make the probabilistic "expected" time complexity to be
  O(n log(n)).

