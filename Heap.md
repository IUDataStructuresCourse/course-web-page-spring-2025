# Lab: Heap

## Background and Overview

The lab is to finish a `Heap` class

## Support Code and Submission

+ Student support code is at [link](https://github.com/IUDataStructuresCourse/Heap-student-support-code/).
+ Submit your code file `Heap.java` 
+ Submit your test file `StudentTest.java` 


## Problem Set

### Problem 1: Heap Implementation

  Your task is to implement the Heap class by completeing the following methods:

  ```java
  Heap(Comparator<E> comparator) 
  ```
  
  The heap constructor containing the keys and comparator.
  

  ```java
  peek()
  ```

  Returns the top of this heap. This will be the lowest priority
  key. This method should throw a `NoSuchElementException` if the heap
  is empty.


  ```java
  push(E key) 
  ```

  Adds a key to the heap, maintaining the heap invariant, placing it
  in the correct priority order.


  ```java
  pop() 
  ```

  Removes and returns the lowest priority key in this heap.  This
  method should throw a `NoSuchElementException` if the heap is empty.

  
  ```java
  swap(int i, int j) 
  ```

  Exchanges the elements in the heap at the given indices in keys.


  ```java
  size()
  ```

  Returns the number of keys in this heap.


  ```java
  getLeft(int p)
  ```

  Returns the index of the left child of p.


  ```java
  getRight(int p)
  ```

  Returns the index of the right child of p.


  ```java
  getParent(int p) 
  ```

  Returns the index of the parent of p.



### Problem 2: Testing

Write test cases in `StudentTest.java` with one method named `test()`.
This method should thoroughly test the `Heap` class.

Test and debug your own code from Problem 1 locally and then submit both your code
and your test cases to Autograder for grading.

The Autograder will apply your tests to buggy implementations of the
`Heap` class. You receive 1 point for each bug detected.
The Autograder will also apply your tests to a correct implementation
to rule out false positives.

-----------------

* You have reached the end of the lab. Yay!
