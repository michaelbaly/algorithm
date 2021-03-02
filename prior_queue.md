# How to implement max-priority queue?
# Use in which scenarios ? --- job scheduler
+ based on max-heaps
+ defination: a priority queue is *data structure* for maintaining a set of
  S of elements, each with an associated value called a *key*
## support operation for max-priority queue
+ INSERT(S, x)
+ MAXIMUM(S)
+ EXTRACT-MAX(S)
+ INCREASE-KEY(S, x, k) --- increase x's key value to k


![](max_heapify.png)
+ Pseudocode
```
MAX-HEAPIFY(A, i)
    l = LEFT(i)
    r = RIGHT(i)
    if l <= A.heap-size and A[l] > A[i]
        /* A[i] should float down to left subtree */
        largest = l
    else largest = i /* A[i] bigger than A[l] */
    /* A[i] should float down to right subtree */
    if r <= A.heap-size and A[r] > A[largest]
        largest = r
    if largest != i
        swap A[i] with A[largest]
        /* update next node root at which need heapify */
        MAX-HEAPIFY(A, largest)
```
### alternatively we got Pseudocode for MIN-HEAPIFY
```
MIN-HEAPIFY(A, i)
    l = LEFT(i)
    r = RIGHT(i)
    if l <= A.heap-size and A[l] < A[i]
        smallest = l
    else smallest = i
    if r <= A.heap-size and A[r] < A[smallest]
        smallest = r
    if smallest != i /* we need a recursive call */
        swap A[i] with A[smallest]
        MIN-HEAPIFY(A, smallest)
```
## Pseudocode
```
HEAP-MAXIMUM(A)
  return A[1]
```
## EXTRACT-MAX can be used to extract next priority jobs to be scheduled
```
HEAP-EXTRACT-MAX(A)
  if A.heap-size < 1
      error "heap underflow" /* heap is empty */
  max = A[1]
  A[1] = A[A.heap-size] /* likely to violate max-heap property */
  A.heap-size --
  MAX-HEAPIFY(A, 1) /* for keeping heap property */
```
```
HEAP-INCREASE-KEY(A, i, key)
  if key < A[i]
      error "new key smaller than current one"
  A[i] = key
  /* swap value pair until A match heap property */
  while i > 1 and A[PARENT(i)] < A[i]
      exchange A[PARENT(i)] with A[i]
      i = PARENT(i) /* move up index */
```
+ correctness:
+ running time: O(logn)

```
MAX-HEAP-INSERT(A. key)
  A.heap-size ++
  A[A.heap-size] = -unlimited
  HEAP-INCREASE-KEY(A, A.heap-size, key)
```
+ correctness:
+ running time: O(logn)

```
HEAP-DELETION(A, i)
  /* max = {A[LEFT(i)], A[RIGHT(i)]}
  A[i] = max - 1 /* replace A[i] with a less value */
  MAX-HEAPIFY(A, i) */
  j = i
  A[j] = -unlimited
  /* trace the index where A[i] goes */
  while j < A.heap-size
      if A[LEFT(j)] < A[RIGHT(j)]
          exchange A[RIGHT(j)] with A[j]
          j = RIGHT(j)
      else
          exchage A[LEFT(j)] with A[j]
          j = LEFT(j)
  /* remove A[j] */
  if A[j].p.left == A[j]
      A[j].p.left = nil
  else A[j].p.right = nil
  A.heap-size = A.heap-size - 1
```
+ running time: O(logn)
## Conclusion: a heap can support any priority-queue operation on a set of size n in O(logn) time.

## regarding to min-heap
## support operations:
+ INSERT
+ MINIMUM
+ EXTRACT-MIN
+ DECREASE-KEY

# HOW to build a min-heap ?
# USE in which scenarios ? --- event-drive simulator

## Pseudocode for operations

```
MIN-HEAP-MINIMUM(A)
  return A[1]
```
## EXTRACT-MIN used to choose next event to for simulate
```
MIN-HEAP-EXTRACT-MIN(A)
  min = A[1]
  A[1] = A[A.heap-size]
  A.heap-size = A.heap-size - 1
  MIN-HEAPIFY(A, 1)
```

```
HEAP-DECREASE-KEY(A, i, k)
    if k >= A[i]
        error "new value should be less than current one"
    A[i] = k
    while i > 1 and A[PARENT(i)] > A[i]
        /* move up lower value */
        exchange A[PARENT(i)] with A[i]
        i = PARENT(i)
```

```
MIN-HEAP-INSERT(A, k)
    A.heap-size = A.heap-size + 1
    A[A.heap-size] = + infinate
    HEAP-DECREASE-KEY(A, A.heap-size, k)
```
