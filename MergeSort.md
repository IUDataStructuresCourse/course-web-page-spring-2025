
Implement and test the Merge Sort algorithm using the `Iterator` and
`Comparable` interfaces. The `Iterator` interface is in the student
support code and `Comparable` is in the Java standard library.  The
support code is in the following github repository.

[https://github.com/IUDataStructuresCourse/merge-sort-student-support-code](https://github.com/IUDataStructuresCourse/merge-sort-student-support-code)

## Merge Sort

Define a public `MergeSort` class in `MergeSort.java` with the
following method:

    public static <E extends Comparable<? super E>>
    void sort(Iterator<E> begin, Iterator<E> end);

The `begin` and `end` iterators represent the half-open range
`[begin,end)` of a sequence. After the call to `sort`, the range
`[begin,end)` should contain the same elements as before, but sorted
from low to high according to the `Comparable` ordering.
The iterators `begin` and `end` should not be mutated, but of course
you can clone them and mutate the clones.

The Merge Sort algorithm relies on the Merge algorithm, which you should
implement with the following method:

    public static <E extends Comparable<? super E>>
    Iterator<E> merge(Iterator<E> begin1, Iterator<E> end1,
	                  Iterator<E> begin2, Iterator<E> end2,
	                  Iterator<E> result);

Prior to the call to `merge`, the input ranges `[begin1,end1)` and
`[begin2,end2)` must be sorted.  Let n be the number of elements in
the first range and m be the number of elements in the second range.
The elements from both ranges are written to the range
`[result,result.advance(n+m))` in such a way that the result is in
sorted order.  The `merge` function should return an iterator for the
position `result.advance(n+m)`.  All of the iterator parameters
(`begin1`, `end1`, etc.) should not be mutated directly, but you can
clone them and mutate the clones.

## Testing

Create file `test/StudentTest.java`, which contains `public class StudentTest`.
Create your unit tests as methods of `test/StudentTest.java`. If you come up
with your own test properties, those functions for test properties should go in
`test/StudentTest.java` as well.

The `StudentTest` class should contain a member function
`public void test()` (marked with `@Test`) which serves as the main entrance.
For example, if you have 2 test functions `test_find_foo()` and `test_find_bar()`,
the `test()` function should be:

```java
@Test
public void test() {
    test_find_foo();
    test_find_bar();
}
```
