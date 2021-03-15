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
1  initiate baseA = 1
2  initiate baseB = 1
3  n = A.length
4  /* base case1: return the 1th smallest value */
5  if k == 1
6      if A[0]>B[0]
7          return B[0]
8      else
9          return A[0]
10  /* base case2: if any A or B reach the boundary, return the first value in other array */
11  if baseA > n
12     return B[baseB]
13  else if baseB > n
14     return A[baseA]
15  mid = k/2
16  if A[mid] < B[mid]
17      /* drop first half k in A[baseA]~A[mid] and update next itera start to A[mid + 1]*/
18      FIND-KTH(A[baseA + mid + 1], B[baseB], k - mid) /* update next drop base to (k - mid) */
19      baseA = baseA + mid + 1
20  else if A[mid] > B[mid]
      /* drop first half k in B[baseB]~B[mid] */
21      FIND-KTH(A[baseA], B[baseB + mid + 1], k - mid)
22      baseB = baseB + mid + 1
23  else
24      return A[mid] /*the kth smallest value */
```
+ correctness: both A and B has length of n. in order to find the kth smallest element, we have a concluesion, if A[k/2] < B[k/2], then A[k/2] is no more than the kth smallest element, cause we have two increasing sorted array, we can draw this conclusion by contradiction, simply saying, we always have (2n - k + 1) elements right to(bigger than) the kth smallest element. so every time we drop the first half from either array, we update next k, for example, the first time we drop k/2, we drop k/4 next time, etc.
+ running time: when k is much less than n, by issuing the recursive FIND-KTH function, it takes O(logk) to find out the kth smallest value, since every time we just drop half of the element in one of the array. but considering the worst case, to say, k likely to reach n, then it takes O(logn) to find the kth smallest value, so in common case, this algorithm has worst running time of O(logn)

# problem6
```
PARTITION(A, p, x)
1  y = x
2  i = p - 1
3  for j = p to r - 1
4      if A[j] <= y
5          i = i + 1
6          exchange A[i] with A[j]
7  exchange A[i+1] with x
8  return i + 1

SELECT(A, p, r, i)
1   if p == 1
       /* divide A of n to floor(n/5) groups, last group will have n mod 5 elements */
2       groups = ceiling(n/5)
3       for j = 1 to groups
4           B[j][5] to save each group elements
5       for j = 1 to groups
6       /* find median of each group and save them in sorted array C */
7       x = SELETC(C, 1, groups, groups/2) /* x represent the element to partition around */
8   if p == r
9       return A[p]
10   q = PARTITION(A, p, x)
11   k = q - p + 1
12   if i = = k
13       return A[q]
14   else if i < k
15       return SELECT(A, p, q - 1, i)
16   else return SELECT(A, q + 1, r, i - k)
```
+ specification: this algorithm follows the procedure proofs in textbook chapter 9.3 which have 5 steps to show.
+ running time: the running time is guaranteed by the recurrence expression in chapter 9.3, which indicates O(n) is the running time.
