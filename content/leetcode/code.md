# LeetCode 148: Sort List

**Method Used:** Top-Down Merge Sort
- **Time Complexity:** $O(n \log n)$
- **Why this method?** It provides the optimal time complexity required for sorting a linked list efficiently.

## Core Steps
1. **Find Middle:** Use slow and fast pointers to find the midpoint of the list.
2. **Split:** Divide the list into two distinct halves.
3. **Sort:** Recursively call the sort function on both halves.
4. **Merge:** Combine the two sorted halves back together using a dummy node.

## Key Code Concepts

### 1. The `tmp` Variable (Splitting the List)
`tmp` acts as a temporary placeholder to safely hold data while modifying pointers. When splitting the list, it prevents the second half from being lost in memory:
* `tmp = right.next` (Save the start of the right half)
* `right.next = None` (Break the physical link between the halves)
* `right = tmp` (Reassign the right pointer to the saved start node)

### 2. Type Hinting: `Optional[ListNode]`
* **Meaning:** It indicates that the variable can either be a `ListNode` object OR it can be `None` (which represents an empty list). 
* **Purpose:** It serves as documentation for developers and allows code editors to warn you if you try to perform operations on a `None` value without checking first.
* **Modern Syntax (Python 3.10+):** `ListNode | None`

* link of mergesort:[https://www.youtube.com/watch?v=4VqmGXwpLqc]

# LRU Cache (LeetCode 146) - Python Notes

**Strategy:** Hash Map (for $O(1)$ lookup) + Doubly Linked List (for $O(1)$ order tracking).

## Core Methods

### 1. `__init__(capacity)`
- Initializes the dictionary (`self.cache`).
- Creates dummy `head` and `tail` nodes.
- **Crucial:** Connect dummy nodes immediately (`head.next = tail`, `tail.prev = head`) to avoid `NoneType` attribute errors during inserts.

### 2. `add(node)`
- Inserts a node right after `self.head` (Most Recently Used position).
- **Safe approach:** Anchor the rest of the list first (`nxt = self.head.next`) before rewiring `prev` and `next` pointers.

### 3. `remove(node)`
- Unlinks a node from its current neighbors in the doubly linked list.
- **Note:** Only modifies list pointers, does *not* erase data from the dictionary.

### 4. `get(key)`
- If key exists: grab the node, `remove(node)`, then `add(node)` to push it to the front (MRU).
- **Crucial:** Return `node.val` (not the key).

### 5. `put(key, value)`
- If key exists: `remove(node)` from its current spot.
- Create new `ListNode`, `add(node)` to the front, and map it in `self.cache`.
- **Eviction Rule:** If `len(cache) > capacity`:
  - Identify LRU: `lru_node = self.tail.prev`
  - 1. Unlink from list: `self.remove(lru_node)`
  - 2. Erase from dictionary: `del self.cache[lru_node.key]`