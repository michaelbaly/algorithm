### Definitions
#### Properties of RBT
+ 1 every node is either red or black
+ 2 the root is black
+ 3 every leaf(NIL) is black
+ 4 if a node is red, then both its children are black
+ 5 for each node, every simple path from the node to its descendant leaves contains same number of black nodes

#### black height of a node x
+ 1 represent: bh(x) --- the number of black node down to a leaf (not include node x)
+ 2 black height of a RBT --- bh(root)

#### lemma 13.1
* the RBT with n internal nodes has height at most **2log(n+1)**
* dynamic-set operations --- SEARCH/MIN/MAX/SUCCESSOR/PREDECESSOR takes **log(n)**
* how to support INSERT/DELETION operation in **log(n)**


![](rotation.png)
```
LEFT-ROTATION(T,x)
  y = x.right
  x.right = y.left                   /* update x's right subtree */
  if y.left != T.nil
    y.left.p = x                     /* link y's left subtree to x */

  y.p = x.p                          /* link x's parent to y */
  if x.p == T.nil                    
    T.root = y                       
  elseif x == x.p.left               
    x.p.left = y
  else x.p.right = y                 

  y.left = x
  x.p = y                            /* update x's parent */
```

```
RIGHT-ROTATION(T,y)
  x = y.left
  y.left = x.right                   /* update y's left subtree */
  if x.right != T.nil
    x.right.p = y                    /* update parent of x's right subtree */

  x.p = y.p                          /* link y's parent to x */
  if y.p == T.nil
    T.root = x
  elseif y == y.p.left
    y.p.left = x
  else y.p.right = x

  x.right = y                       /* update y's parent */
  y.p = x
```

```
RB-INSERT(T, z)
  y = T.nil
  x = T.root
  while x != T.nil
      y = x
      if z.key < x.key
          x = x.left
      else x = x.right

  z.p = y           /* link z to y's subtree */
  if y == T.nil     /* T.root is nil */
      T.root = z
  else z.key < y.key
      y.left = z
  else y.right = z

  z.left = nil     /* maintain proper tree s */
  z.right = nil
  z.color = RED
  RB-INSERT-FIXUP(T, z)
```

```
RB-INSERT-FIXUP(T, z)
  while z.p.color == RED
      if z.p == z.p.p.left /* z's parent lies in its grandparents left subtree */
          y = z.p.p.right  /* z's uncle */
          if y.color == RED    /* CASE1 */
              z.p.color = BLACK
              y.color = BLACK
              z.p.p.color = RED
              z = z.p.p    /* new position for next iteration */
          else if z == z.p.right  /* CASE2 */
              z = z.p
              LEFT-ROTATION(T, z)
          z.p.color = BLACK       /* CASE3 */
          z.p.p.color = RED
          RIGHT-ROTATION(T, z.p.p)
      else /* right subtree */
          ...
          ...
  T.root.color = BLACK
```
