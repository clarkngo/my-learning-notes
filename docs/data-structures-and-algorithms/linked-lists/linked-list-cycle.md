---
title: Linked List Cycle
layout: home
parent: Linked Lists
---

## Linked List Cycle

Given `head`, the head of a linked list, determine if the linked list has a cycle in it.

There is a cycle in a linked list if there is some node in the list that can be reached again by continuously following the `next` pointer. Internally, `pos` is used to denote the index of the node that tail's `next` pointer is connected to. **Note that `pos` is not passed as a parameter.**

Return `true` if there is a cycle in the linked list. Otherwise, return `false`.

---

**Example 1:**
* **Input:** `head = [3,2,0,-4]`, `pos = 1`
* **Output:** `true`
* **Explanation:** There is a cycle in the linked list, where the tail connects to the 1st node (0-indexed).

**Example 2:**
* **Input:** `head = [1,2]`, `pos = 0`
* **Output:** `true`
* **Explanation:** There is a cycle in the linked list, where the tail connects to the 0th node.

**Example 3:**
* **Input:** `head = [1]`, `pos = -1`
* **Output:** `false`
* **Explanation:** There is no cycle in the linked list.

---

**Constraints:**
* The number of the nodes in the list is in the range `[0, 10^4]`.
* `-10^5 <= Node.val <= 10^5`
* `pos` is `-1` or a **valid index** in the linked-list.

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def hasCycle(self, head: Optional[ListNode]) -> bool:
        seen = set()
        cur = head
        while cur:
            if cur in seen:
                return True
            seen.add(cur)
            cur = cur.next
        return False
```            

* **Time Complexity: $O(n)$**
    * The algorithm iterates through the linked list once. In the worst case (no cycle), it visits all $n$ nodes.
    * Each operation inside the loop (checking if an item is in the set and adding to the set) takes $O(1)$ average time.
    * Therefore, the total time is proportional to $n$.

* **Space Complexity: $O(n)$**
    * The `seen` set stores each node as it's visited.
    * In the worst case (no cycle), the set will grow to store all $n$ nodes of the list.
    * Therefore, the space required is proportional to $n$.