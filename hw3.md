### problem 1
+ desc: x = a + b where a,b belongs two array A, B sizeof n (not sorted)
+ requirement: O(nlogn)
```
ELEMENT-EXIST(A, B, x)
  HEAPSORT(B)
  for i = 1 to n
      y = x - A[i]
      exist = BIN-SEARCH(B, 1, n, y)
      if exist
          return true
  return false
```
```
BIN-SEARCH(B, begin, end, y)
  mid = (begin + end)/2
  if B[mid] == y
    return mid
  if begin >= end
    return -1
  if B[mid] < y
    return BIN-SEARCH(B, mid + 1, end, y)
  if B[mid] > y
    return BIN-SEARCH(B, begin, mid - 1, y)
```
+ running time:
*1 heapsort for array B cost O(nlogn)
*2 it takes n times at most to walk array A, and BIN-SEARCH takes O(logn) at most, so line7~line12 will takes at most O(nlogn) time to find whether there exist such a value pair
*3 thus, running time is O(nlogn)

### problem 2
+ recurrence

### problem 3
+ show running time thelta(n^2) if array has decreasing order
+ example A = [20 19 17 15 13 10 9 7 6 4 3 1]
+ we can see array is sorted in decreasing order, on first partition, we choose 1 as pivot, then we generate two subarray with 0 element and n-1 elements, subarray with 0 element will immediately return, thus cost
thelta(1), and partition on subarray [19 17 15 13 10 9 7 6 4 3 1 20] will also generate two subarray of size 0 and n - 2 representatively. we know partition operation cost thelta(n) each time, so the recurrence will apply as T(n) = T(n-1)+thelta(1)+thelta(n) if unbalanced partition happens every time, with the substitution method, we know the running time is T(n) = thelta(n^2)

### problem 4
```
K-CLOEST-ELEMENTS(S, k)
  median = (1+n)/2 //n is odd
  // build hash table for set S of size n, which takes thelta(n)
  j = SELECT(S, 1, n, i)
  value = HASH-SEARCH(T, j)
  if value == S[median]
      // then we know S[median] is the ith smallest element in the set
      left = SELECT(S, 1, n, i-1)
      right = SELECT(S, 1, n, i+1)
  // save the smaller element the (i-1) and (i+1) smallest in array B
  interval_l = |S[left] - S[median]|
  interval_r = |S[right] - S[median]|
  if interval_l > interval_r
      B[indies++] = S[right]
      right = SELECT(S, 1, n, i+1)
  else interval_l < interval_r
      B[indies++] = S[left]
      left = SELECT(S, 1, n, i+1)
```
