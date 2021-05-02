### BFS algorithm
```
BFS(G, s)
  for each v in G.V - {s}
      v.color = WHITE
      v.d = infinity
      v.parent = NIL
  s.color = GRAY
  s.d = 0
  s.parent = NIL
  Q = empty
  ENQUEUE(Q, s)
  while Q != empty
      u = DEQUEUE(Q)
      for each v in G.Adj[u]
          if v.color == WHITE
              v.color = GRAY
              v.d = u.d + 1
              v.parent = u  //use to trace a shortest path
              ENQUEUE(Q, v) //loop iterates as long as there remain gray vertices
      u.color = BLACK
```
* running time:
+ 1 queue operation takes O(V) --- > overall, every veritces will be gray, and queue operation happens only in gray vertices.
+ 2 initialization cost O(V) for all vertices
+ 3 since the sum of the length of all adjacency lists is thelta(E), then total time spend in scanning is O(E)
+ 4 thus total running time is **O(V + E)**

### breadth first trees
```
PRINT-PATH(G, s, v) // print the edge from vertice s to v
  if s == v.parent
      print s
      return
  else if NIL == v.parent
      print "no path from s to v"
      return
  PRINT-PATH(G, s, v.parent)
  print v
```

### Depth-first search
```
DFS(G)
  for each vertex v in G.V
      v.color = WHITE
      v.parent = NIL
  time = 0
  for each vertex v in G.V
      if v.color = WHITE
          DFS-VISIT(G, v)
```
```
DFS-VISIT(G, v)
  time = time + 1
  v.d = time
  v.color = GRAY
  for each vertice u in G.Adj[v] // explore edge (v, u)
      if u.color = WHITE
          u.parent = v
          DFS-VISIT(G, u)
  v.color = BLACK
  time = time + 1
  v.f = time
```
### Topological sort
```
TOPOLOGICAL-SORT
  call DFS(G) to compute finish time v.f for each vertex v
  as each vertex is finished, insert it into front of a linked list
  return linked list of all vertices  
```
### minimal spanning tree
+ the generic thought: manages a set of edges A, A keep the invariant (do not forms a circle in each iteration)

```
GENERIC-MST(G, w)
  A = empty
  while A does not forms a circle
      find an edge of (u,v) that's safe for A // carefully considering saft edge
      A = A U {(u,v)} //(u,v) is safe edge for A
  return A
```

+ **light edge** --- an edge with minimal weight that crosses a cut (V-S, S)
+ a light edge satisfying a given properties
+ Thoerem 23.1 --- if (u,v) is a light edge, for a cut (S, V-S), then edge (u,v) is safe for A
+ corollary 23.2 --- let C = (Vc, Ec) be a connected component (tree) in GA = (V, A), if (u,v) is a light edge connect C to someother component in GA, then (u,v) is safe for A

### kruskal's MST --- use disjoint data struct to represent a forest
```
MST-KRUSKAL(G, w)
1  A = empty
2  for each vertex v in G.V
3     MAKE-SET(v)
4  sort the edges of G.E into nondecreasing order by weight w
5  for each edge (u,v) in G.E, taken in nondecreasing order by weight
6      if FIND-SET(u) != FIND-SET(v)
7          A = A U {(u,v)}
8          UNION(u,v)
9  return A
```
### prim's MST
+ 1 set of A
* start : A = {(v,v.parent), v in G - {r} - Q}
* end   : A = {(v,v.parent), v in G - {r}}
```
MST-PRIM(G, w, r)
  for each u in G.V
      u.value = infinity
      v.parent = NIL
  r.value = 0 //arbitery root vertice
  Q = G.V
  while Q != NIL
      u = EXTRACT-MIN(Q)
      for each v in G.Adj[u]
          if v in Q & w(u,v) < v.value
              v.value = w(u,v)  // looks much like DECREASE-KEY operation
              v.parent = u
```
* if edge weight is not distinct, there may exist **multiple MST** --- considering edge (a,h) to construct another MST with weight 37 in Fig 23.5

## single-source shortest path
```
INITIALIZE-SINGLE-SOURCE(G, s)
  for each v in G.V
      v.d = infinity
      v.parent = NIL
  s.d = 0
```
* relaxation is used to considering whether update v.d by v.d <= u.d + w(u,v). if yes, v.d is **relaxed** to remain unchanged. you can think v.d is **still alive**
```
RELAX(u, v, w)
  if v.d > u.d + w(u,v)
      v.d = u.d + w(u,v)
      v.parent = u
```
```
DAG-SHORTEST-PATHS(G, w, s)
  TBD
```
+ the DAG-SHORTEST-PATHS differ when choose different source s --- exercise 24.2-1

### Dijkstra's algorithm --- for case in which all weight are non-negative
