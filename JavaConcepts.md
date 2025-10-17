1. TreeMap

Internal Implementation: Red-Black Tree (Self-balancing BST)

Ordering: Sorted by keys (natural order or custom Comparator)

Time Complexity:

get() / put() / remove() → O(log N)
➤ Because it must traverse the tree height (log N) and may rebalance nodes after insertions/deletions.

When to Use: When you need sorted keys or range queries (headMap, tailMap)

Useful Methods:

firstEntry(), lastEntry(), ceilingEntry(), floorEntry(), subMap()
