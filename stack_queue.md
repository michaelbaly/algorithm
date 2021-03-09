## operations on stack

```
STACK-EMPTY(S)
  if S.top == 0
    return true
  else
    return false
```
```
PUSH(S, x)
  S.top = S.top + 1
  S[S.top] = x
```
```
POP(S)
  if STACK-EMPTY(S)
      error "underflow"
  else
     S.top = S.top - 1
     return S[S.top + 1]
```
```
FIND-MIN(S)
  min = S[S.top]
  for i = S.top - 1 to 1
      if S.top < min
          min = S[S.top]
  return min
```
+ running time: S.top is a constant value assming N. it will take N*O(1) ~ O(1)

## operations on queue
+ question: why do not implement n elements in array A[1...n]???
+ ![](image/queue.png)
```
ENQUEUE(Q, x)
  Q[Q.tail] = x
  if Q.tail = Q.length
      Q.tail = 1
  else
      Q.tail = Q.tail + 1
```
```
DEQUEUE(Q)
  x = Q[Q.head]
  if Q.head = Q.length
      Q.head = 1
  else
      Q.head = Q.head + 1
  return x
```
