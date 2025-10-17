### Collections

#### TreeMap

- Internal Implementation: Red-Black Tree (Self-balancing BST)
- Ordering: Sorted by keys (natural order or custom Comparator)
- Time Complexity:
         - get() / put() / remove() → O(log N)
         - Because it must traverse the tree height (log N) and may rebalance nodes after insertions/deletions.
- When to Use: When you need sorted keys or range queries (headMap, tailMap)
- Useful Methods: firstEntry(), lastEntry(), ceilingEntry(), floorEntry(), subMap()


#### HashMap

- Internal Implementation: Array of buckets + linked list / balanced tree (after JDK 8)
- Ordering: Unordered
- Time Complexity:
         - Average: get() / put() / remove() → O(1)
         - Because hash function maps keys directly to an array bucket, requiring constant-time access.
         - Worst-case: O(log N) : If too many keys collide in the same bucket (converted to a Red-Black Tree).
- When to Use: For fast key–value lookups where ordering doesn’t matter
- Allows one null key and multiple null values
- Load factor (default 0.75) controls rehashing frequency

#### LinkedHashMap

- Internal Implementation: HashMap + Doubly Linked List across entries
- Ordering: Insertion-order by default
- Access-order if accessOrder=true in constructor
- Time Complexity:
        - get() / put() / remove() → O(1)
        - Because lookup is still hash-based; linked list only adds lightweight ordering pointers.
- When to Use: When you need predictable iteration order or LRU cache behavior
- Useful Methods: Override removeEldestEntry() for auto-removal of old entries.

#### HashSet

- Internal Implementation: Backed by a HashMap (values stored as dummy objects)
- Ordering: Unordered
- Time Complexity:
        - add() / remove() / contains() → O(1) average
        - Because operations use hash-based indexing just like HashMap.
- When to Use: When you need unique elements and fast lookups

#### LinkedHashSet

- Internal Implementation: Backed by LinkedHashMap
- Ordering: Insertion-order
- Time Complexity:
         - add() / remove() / contains() → O(1) average
         - Same as HashSet; only extra cost is maintaining linked order.
- When to Use: When you want unique elements but also care about insertion order

#### TreeSet

- Internal Implementation: Backed by a TreeMap
- Ordering: Sorted (natural or custom Comparator)
- Time Complexity:
        - add() / remove() / contains() → O(log N)
        - Because it navigates a balanced tree structure and may rebalance after updates.
- When to Use: When you need sorted unique elements
- Useful Methods: first(), last(), higher(), lower(), subSet()


#### PriorityQueue

- Internal Implementation: Binary Heap (usually min-heap)
- Ordering: Elements ordered by natural order or custom Comparator
- Time Complexity:
        - offer() / poll() → O(log N)
        - Because the heap property must be restored after insertion/removal.
        - peek() → O(1)
        - Top element is always stored at the root (index 0).
- When to Use: For min-heap / max-heap behavior, scheduling, or top-K problems

#### ArrayList

- Internal Implementation: Dynamic Array
- Ordering: Maintains insertion order
- Time Complexity:
      - Random access → O(1)
      - Direct index access in an array.
      - Insert/remove (middle) → O(N)
      - Requires shifting elements to maintain order.
- When to Use: For fast random access and infrequent insertions/deletions


#### LinkedList

- Internal Implementation: Doubly Linked List
- Ordering: Maintains insertion order
- Time Complexity:
        - Add/remove at ends → O(1)
        - Just update a few node pointers.
        - Random access → O(N)
        - Must traverse nodes sequentially to reach index.
- When to Use: For frequent insertions/deletions or queue/deque operations

#### ConcurrentHashMap

- Internal Implementation: Hash Segments / CAS-based synchronization (JDK 8+)
- Ordering: Unordered
- Time Complexity:
        - Average get() / put() / remove() → O(1)
        - Because buckets are independent and locking is fine-grained (reducing contention).
- When to Use: For thread-safe concurrent access without blocking the whole map

