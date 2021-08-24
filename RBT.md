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
  z.color = RED    /* maintain property 5, no impact on BH(black height) */
  RB-INSERT-FIXUP(T, z)
```

```
RB-INSERT-FIXUP(T, z)
  while z.p.color == RED
      if z.p == z.p.p.left /* z's parent lies in its grandparents left subtree */
          y = z.p.p.right  /* z's uncle */
          if y.color == RED            /* CASE1 */
              z.p.color = BLACK
              y.color = BLACK
              z.p.p.color = RED
              z = z.p.p    /* new position for next iteration */
          else if z == z.p.right       /* CASE2 */
              z = z.p
              LEFT-ROTATION(T, z)
          else
              z.p.color = BLACK        /* CASE3 */
              z.p.p.color = RED
              RIGHT-ROTATION(T, z.p.p)
      else /* right subtree */
          y = z.p.p.left  /* uncle lies grandparents's left subtree */
          if y.color == RED
              z.p.color = BLACK
              y.color = BLACK
              z.p.p.color = RED
              z = z.p.p /* new position for next iteration */
          else if z == z.p.left
              z = z.p
              RIGHT_ROTATION(T, z)
          else
              z.p.color = BLACK
              z.p.p.color = RED
              LEFT_ROTATION(T, z.p.p)
  T.root.color = BLACK
```

#### Delete operation
```
RB-TRANSPLANT(T, u, v)
  if u.p == T.nil
      T.root = v
  else if u = u.p.left
      u.p.left = v
  else u.p.right = v

  u.p = v.p
```
```
RB-DELETION(T, z)
  y = z
  y-orig-color = y.color
  if z.left == T.nil
      x = z.right
      RB-TRANSPLANT(T, z, z.right)
  else if z.right == T.nil
      x = z.left
      RB-TRANSPLANT(T, z, z.left)
  else
      y = TREE-MINIMUM(z.right)
      y-orig-color = y.color
      x = y.right
      if y.p == z
          x.p = y
      else
          RB-TRANSPLANT(T, y, y.right)
          y.right = z.right
          y.right.p = y
      RB-TRANSPLANT(T, z, y)
      y.left = z.left
      y.left.p = y
      y.color = z.color
  if y-orig-color == BLACK
      RB-DELETE-FIXUP(T, x)
```
#### restore RBT's Properties caused by RB-DELETION
```
RB-DELETE-FIXUP(T, x)
  while x != T.root and x.color == BLACK
      if x == x.p.left
          w = x.p.right
          if w.color == RED
              w.color = BLACK
              x.p.color = RED
              LEFT_ROTATION(T, x.p)
              w = x.p.right
          if w.left.color == BLACK and w.right.color == BLACK
              w.color = RED
              x = x.p
          else if w.right.color == BLACK
              w.left.color = BLACK
              w.color = RED
              RIGHT-ROTATE(T, w)
              w = x.p.left
          else
              w.color = x.p.color
              x.p.color = BLACK
              w.right.color = BLACK
              LEFT-ROTATE(T, x.p)
              x = T.root
      else
          ...
          ...
  x.color = BLACK
```
