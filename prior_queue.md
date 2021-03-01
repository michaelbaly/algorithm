# How to implement max-priority queue?
+ based on max-heaps
+ defination: a priority queue is *data structure* for maintaining a set of
  S of elements, each with an associated value called a *key*
## support operation for max-priority queue
+ INSERT(S, x)
+ MAXIMUM(S)
+ EXTRACT-MAX(S)
+ INCREASE-KEY(S, x, k) --- increase x's key value to k

## Pseudocode
```
HEAP-MAXIMUM(A)
  return A[1]
```

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
