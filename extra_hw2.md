# problem1
+ this problem is aim to find the index pair (i, j) which meet below formular
+ max-interval = max_j - min_i where 1<=i<j<=n
+ max_j = max{Aj, Aj+1, ..., An} and max_i = min{A1, A2, ..., Ai}
+ and the question is to mind every max and min value in each two partition array.
+ then we have the recursive relation
(j,i) = max{Aj,...,An} - min{A1,...,Ai} where 1<=i<j<=n
+ then we have our pseudocode
```
MAX-INTERVAL-PAIR(A)
  //let set S to keep the index pair (i, j) and
  //array B to keep current max interval
  //let lp_min[i] store the minimal vale from A1,...,Ai
  lp_min[1] = A[1]
  for i = 1 to n
      lp_min[i] = min(lp_min[i], A[i])
  //let rp_max[j] store the maximual vale from Aj,...,An
  rp_max[n] = A[n]
  for j = n downto 1
      rp_max[j] = max(rp_max[j], A[n])
  for k = 1 to n // k is where we partition the array
      cur_max_interval = rp_max[n-k] - lp_min[k]
      j = index of rp_max
      i = index of lp_min
      B <- current_max_interval
      S <- (i,j)
  max = B[1]
  for k = 2 to n-1
      if B[k] > max
          max = B[k]
          (i,j) = S[k]
  return (i,j)
```   
#### running time:
* 1 line-line two for loop, takes O(n) time to record left min and right max with element index
* 2 line-line takes O(n) to calculate max interval for each subproblem
* 3 line-line take thelta(n) to find the max interval of all subproblem and index pair
* 4 thus it takes O(n) for this algorithm
#### correctness:
the subproblem is to find max interval in each partition, after we record all the subproblem, and we can find the optimal solution for the value pair.

# problem2
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
