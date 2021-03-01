![](rotation.png)
```
LEFT-ROTATION(T,x)
  y = x.right
  x.right = y.left                   /* update x's right subtree */
  if y.left != T.nil
    y.left.p = x                     /* link y's left subtree to x */
  y.p = x.p                          /* link x's parent to y */
  if x.p == T.nil                    /* x's parent is root node */
    T.root = y                       /* root node link to y */
  elseif x == x.p.left               /* x node on it's parent's left subtree */
    x.p.left = y
  else x.p.right = y                 /* y linked to which side of x's parent */
  y.left = x
  x.p = y                            /* update x's parent */
```

```
RIGHT-ROTATION(T,y)
  x = y.left
  y.left = x.right                   /* update y's left subtree */
  if x.right != T.nil
    x.right.p = y                    /* update parent of x's right subtree */
  x.p = y.p
  if y.p == T.nil
    T.root = x
  elseif y == y.p.left
    y.p.left = x
  else y.p.right = x
  x.right = y
  y.p = x
```
