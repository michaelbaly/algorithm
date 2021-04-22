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
