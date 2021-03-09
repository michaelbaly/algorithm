## linear scan of increasing sorted array
+ requirement: if counting ties, return the bigger value
+ INPUT example A = {1,2,2,33,56,77,77,99}
+ OUTPUT 77

```
LINEAR-SCAN(A)
  for i = 1 to A.len
      j = i + 1
      if A[j] == A[i]
```
