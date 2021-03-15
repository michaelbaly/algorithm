```
INORDER-TREE-WALK(x)
  if x != nil
      INORDER-TREE-WALK(x.left)
      print x.key
      INORDER-TREE-WALK(x.right)
```
## recursion version
```
TREE-SEARCH(x,k)
  if (x == nil || k == x.key)
      return x
  if k < x.key
      TREE-SEARCH(x.left, k)
  else TREE-SEARCH(x.right, k)
```
## iterative version
```
INTERATIVE-TREE-SEARCH(x,k)
  while x != nil and k != x.key
      if k < x.key
          x = x.left
      else x= x.right
  return x
```
## find min
```
TREE-MINIMUM(x)
  while x.left != nil
      x = x.left
  return x
```
## find max
```
TREE-MAXIMUM(x)
  while x.right != nil
      x = x.right
  return x
```
## find successor
### we can assuming that node on right subtree of BST may not have a successor
```
TREE-SUCCESSOR(x)
  if x.right != nil
      return TREE-MINIMUM(x.right)
  y = x.p
  while y != nil and x == y.right /* until x is y's left node */
      x = y
      y = y.p
  return y  /* find or nil */
```
## find predecessor
```
TREE-PREDECESSOR(x)
  if x.left != nil
      return TREE-MAXIMUN(x.left)
  y = x.p
  while y != nil and x == y.left
      x = p
      y = y.p
  return y /* find or nil */

```
## tree insert
```
TREE-INSERT(T, z)
  y = nil
  x = T.root
  while x != nil
      y = x
      if z.key < x.key
          x = x.left
      else x = x.right
  z.p = y /* link z's parent to y */
  /* link z to specific postion */
  if y == nil
      T.root = z
  elseif z.key < y.key
      y.left = z
  else y.right = z
```
## tree deletion
```
TREE-DELETION(T, z)
  TBD
```
