# problem1

## I have two Stack --- NSatck and MStack, NStack for normal stack and MStack for
   minimal stack
```
MYPUSH(x)
  if STACK-EMPTY(MS)
      PUSH(MS, x)
  else if x < FINDMIN(MS)
      PUSH(MS, x)
  PUSH(NS, x)
```
```
MYPOP()
  if STACK-EMPTY(NS)
      error "stack underflow"
  value = POP(NS)
  if value == FINDMIN(MS)
      POP(MS)
```
```
FINDMIN(S)
  if STACK-EMPTY(S)
      error "stack underflow"
  return S.top
```
+ running time:
PUSH and POP operation takes constant time, so MYPUSH and MYPOP takes constant time; every time we POP normal stack, we'll compare with top of minimal stack,
and top value in minimal stack will always be the minimal for current normal stack. thus FINDMIN takes constant time to return the minimal value.
+ correctness of FINDMIN: every time we push/pop, we'll update/remove current minimal value if it meet the requirement, which guarantee top value on minimal stack always indicate the smallest for normal stack

# problem2
+ candidates: k < n (S.length = n)
+ vote: n approximately to positive infinite

```
CANDIDATES-TO-WIN(S)
  initialize RBT of T(assuming node has two field- id and vote)
  for i = 1 to n
      node-i.id = S[i]
      node-i.vote = 1
      node = RB-SEARCH(T, node-i)
      if node.id != node-i.id
          RB-INSERT(T, node-i)
      else
          node-i.vote = node-i.vote + 1
  max-vote = 1
  INODER-TREE-WALK(T)
      INODER-TREE-WALK(T.left)
      if T.vote > max-vote
          max-vote = node.vote
          winner = node.id
      INODER-TREE-WALK(T.right)
  return winner
```
+ running time: RB-SEARCH takes logk cause RBT have k node at most. so line 38~45 takes nlogk; cause T only have k node, INORDER-TREE-WALK takes logk,
line47~53 takes logk total running time is (n+1)logk,
The final running time should be O(nlogk).

# problem3
```
PATH-PRINT(T, x, y)
  /* height of tree is h = ceiling(logn + 1)*/
  node1 = x.p
  node2 = y.p
  while node1 != root || node2 != root
      if node1.aux == 1 //marked before, this node should be LCA
          lca = node1
          break
      else if node2.aux == 1
          lca = node2
          break
      node1.aux = 1
      node2.aux = 1
      node1 = node1.p
      node2 = node2.p
      if node1.val == node2.val // x and y have same height from LCA
          lca = node1 or node2
          break
  node = x.p
  while node != lca
      output node.val
      node.aux = 0
  node = y.p
  while node != lca
      reverse output node.val
      node.aux = 0
```
+ running time:
  the first traverse who marked lca will be checked by next traverse, which means next traverse comes, the whole traverse will be terminated. so line65~line78 will run k times in total, then running time should be O(k), and line80~line86 will run k times in total, so the final time is O(k) + O(k), thus will be O(k)

# problem4
```
MAX-INTERVAL-DISJOINT(A, B)
  /* use 2D array ARR to store A,B */
  for i = 1 to n /* takes O(n) */
      ARR[1][i] = A[i]
      ARR[2][i] = B[j]
  /* merge sort ARR[1][], it takes O(nlogn) */
  MERGE-SORT(ARR[1], 1, ARR[1].length) /* also MERGE ARR[2] in MERGE procedure */
  start = ARR[1][1]
  interval = ARR[2][1] - ARR[1][1]
  end = start + interval
  for i = 2 to n
      if ARR[1][i] <= start + interval /* not disjoint */
          /* update right boundary with ARR[2][i] if it bigger than start + interval */
          if ARR[2][i] > start + interval
              end = ARR[2][i]
              interval = end - start
          else
              end = start + interval
      else /* disjoint */
         PUSH(C, start)
         PUSH(D, end)
         start = ARR[1][i]
         interval = ARR[2][i] - start
```
+ running time:
1. it takes O(nlogn) to sort ARR
2. line103~line115 take linear time O(n)to compute max interval disjoint set
3. total time will be O(nlogn)

# problem5
```
FIND-KTH(A, B, k) // A and B have same length of n
  initiate baseA = 1
  initiate baseB = 1
  n = A.length
  /* base case1: return the 1th smallest value */
  if k == 1
      if A[0]>B[0]
          return B[0]
      else
          return A[0]
  /* base case2: if any A or B reach the boundary, return the first value in other array */
  if baseA > n
     return B[baseB]
  else if baseB > n
     return A[baseA]
  mid = k/2
  if A[mid] < B[mid]
      /* drop first half k in A[baseA]~A[mid] and update next itera start to A[mid + 1]*/
      FIND-KTH(A[baseA + mid + 1], B[baseB], k - mid) /* update next drop base to (k - mid) */
      baseA = baseA + mid + 1
  else if A[mid] > B[mid]
      /* drop first half k in B[baseB]~B[mid] */
      FIND-KTH(A[baseA], B[baseB + mid + 1], k - mid)
      baseB = baseB + mid + 1
  else
      return A[mid] /*the kth smallest value */
```
+ correctness: both A and B has length of n. in order to find the kth smallest element, we have a concluesion, if A[k/2] < B[k/2], then A[k/2] is no more than the kth smallest element, cause we have two increasing sorted array, we can draw this conclusion by contradiction, simply saying, we always have (2n - k + 1) elements right to(bigger than) the kth smallest element. so every time we drop the first half from either array, we update next k, for example, the first time we drop k/2, we drop k/4 next time, etc.
+ running time: when k is much less than n, by issuing the recursive FIND-KTH function, it takes O(logk) to find out the kth smallest value, since every time we just drop half of the element in one of the array. but considering the worst case, to say, k likely to reach n, then it takes O(logn) to find the kth smallest value, so in common case, this algorithm has worst running time of O(logn)

# problem6
```
SELECT(A,p,q,i)
  /* divide A of n to floor(n/5) groups, last group will have n mod 5 elements */
  
  PARTITION(A,p,q,r)

```
