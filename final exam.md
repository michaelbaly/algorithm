### Problem1


+ 1 In this graph, we have edges (s,v)(s,u)(v,u)(v,w)
+ 2 w(s,v) has negative weight -10, the total weight that contains it has minimal weight of -10+(-3)+4 = -9, however, the set contains all the vertices form a circle, so that it's not a tree.


### Problem1
+ 1. let assume we have a target value V needs to make changes
+ 2. for the minimal number n, we can build a table for the max number our V can holds. Assuming table max_coin holds the max value for each type.
+ 3. thus we can build our recurrent relation(di,ni,qu,pe for type)
     n = min    {di_v, ni_v, qu_v, pe_v} where
       1<=V<=N
     di_v =  (V/DI) + max(ni_v) (1<=V<=N)
     ni_v =  (V/NI) + max(qu_v) (1<=V<=N)
     qu_v =  (V/DI) + max(pe_v) (1<=V<=N)
     pe_v =  (V/NI) (1<=V<=N)
+ 4. thus we have our Pseudocode
```
MOST-CHANGES-NUMBER(V)
     t = initial from max_coin table that indicate which coin holds the max number
     v = V
1    while (v) // t represent coin type
2        // pick the minimal number for the moment
3        n = min {di_v, ni_v, qu_v, pe_v}
4        v = v/n
5        min = min + n
6    return min
```
```
// for k denominations, we need build a tale for k-max-coin, the basic idea follows the same above
DP-K-DENO(V, k)
     t = initial from max_coin table that indicate which coin holds the max number
     v = V
1    while (v) // t represent coin type
         choose the type from k-max-coins table that fit the initial value
         which takes O(k) at most
2        // pick the minimal number for the moment
3        n = min {di_v, ni_v, qu_v, pe_v}
4        v = v/n
5        min = min + n
6    return min
```
### Problem3

#### Pseudocode
```
VERTICES-INDEGREE(G)
1     let array VIN keep indegree number for each vertices
2     Q = G.V // specify a source
      G.V.flag = true
3     while Q != empty
          if u.flag == true
4             u = dequeue(Q)
5         for each vertices v in G.Adj[u]
6             VIN[v] = VIN[v] + 1    
7             if G.Adj[v] != empty  
8                 v.flag = false
9             else
10                v.flag = true
11    return VIN
```

### running time
+ 1 queue operation takes O(V), and O(E) for traverse
+ 2 thus total running time is O(V+E)
