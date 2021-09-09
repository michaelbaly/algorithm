### General Order Statistic
#### Tip: based on RBT

##### Dynamic order statistic

* Retrieving an element with a given rank
* To find the node with the ith smallest key in an order-statistic tree T, we call **OS-SELECT(T.root, i)**
* [Tips]T.NIL.size = 0, so x.left.size or x.right.size equals 0 when x is one level above leaf
```
OS-SELECT(x, i)
  rank = x.left.size + 1
  if rank == i
      return x
  else if rank > i // find in x's left subtree
      OS-SELECT(x.left, i)
  else
      OS-SELECT(x.right, i - rank) // find in x's right subtree
```
```
NONREC-OS-SELECT(x, i)
  r = x.left.size + 1
  y = x
  new-i = i
  while new-i != r && y != NIL
      if i < r
          y = y.left
          r = y.size + 1
      else
          y = y.right
          new-i = i - r
          r = y.left.size + 1

  return y.p
```
* running time: each recursive call go down one level in an order-statistic tree, thus cost O(logn)

* Determining the rank of an element
```
OS-RANK(T, x)
  r = x.left.size + 1
  y = x
  while y != T.root
      if y == y.p.right
          r += y.p.left.size + 1
      y = y.p
  return r
```
* running time: O(logn) as OS-SELECT

### Interval Tree

* concept: An interval tree is a RBT that maintains a dynamic set of elements, with each element x containing an interval x.int
* operation: INT-INSERT/INT-DELETE/INT-SEARCH
* Theorem 14.4 Any execution of INT-SEARCH(T, i) either returns a node whose interval overlaps i, or return T.nil and the tree T contains no node whose interval overlaps i.

![](image\interval-tree.png)
