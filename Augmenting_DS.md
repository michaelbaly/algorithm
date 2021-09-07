### General Order Statistic
#### Tip: based on RBT

##### Dynamic order statistic

* Retrieving an element with a given rank
* To find the node with the ith smallest key in an order-statistic tree T, we call **OS-SELECT(T.root, i)**
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
