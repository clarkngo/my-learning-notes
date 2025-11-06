---
title: Reverse Linked List
layout: default
parent: Linked Lists
---

## Reverse Linked List

Given the `head` of a singly linked list, reverse the list, and return *the reversed list*.

**Example 1:**
* **Input:** `head = [1,2,3,4,5]`
* **Output:** `[5,4,3,2,1]`

**Example 2:**
* **Input:** `head = [1,2]`
* **Output:** `[2,1]`

**Example 3:**
* **Input:** `head = []`
* **Output:** `[]`

**Constraints:**
* The number of nodes in the list is in the range `[0, 5000]`.
* `-5000 <= Node.val <= 5000`

## Solution
```python
class Solution:
    def reverseList(self, head: Optional[ListNode]) -> Optional[ListNode]:
        prev = None
        curr = head

        while curr:
            temp = curr.next
            curr.next = prev
            prev = curr
            curr = temp
        return prev
```

## Code with Comments

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next

class Solution:
    def reverseList(self, head: Optional[ListNode]) -> Optional[ListNode]:
        # Initialize 'prev' to None. This will track the node *before* 'curr'.
        # It will also become the new head of the reversed list.
        prev = None
        
        # Initialize 'curr' to 'head'. This is the node we are currently processing.
        curr = head

        # Loop as long as we haven't fallen off the end of the list
        while curr:
            # 1. SAVE THE NEXT NODE
            # We must store the *next* node in a temporary variable.
            # If we don't, we'll lose the rest of the list when we re-assign curr.next.
            temp = curr.next
            
            # 2. REVERSE THE POINTER
            # This is the actual reversal. We make the current node's 'next' pointer
            # point *backward* to the 'prev' node.
            curr.next = prev
            
            # 3. ADVANCE 'prev'
            # Move 'prev' one step forward. It now points to the node we just processed.
            prev = curr
            
            # 4. ADVANCE 'curr'
            # Move 'curr' one step forward to the *next* node in the *original* list
            # (which we saved in 'temp').
            curr = temp
        
        # When the loop finishes, 'curr' is None, and 'prev' is pointing to
        # the last node we processed, which is the new head of the reversed list.
        return prev
```

This code iteratively reverses a singly-linked list. The core idea is to walk through the list one node at a time, "flipping" the `next` pointer of each node to point to the node that came *before* it.

Here is the time and space complexity for the iterative solution you provided.

---

### Time Complexity: O(n)

The time complexity is **O(n)**, where `n` is the number of nodes in the list.

**Reasoning:**
The algorithm uses a single `while curr:` loop. This loop runs exactly one time for every node in the linked list.

Inside the loop, all operations (assigning `temp`, and re-pointing `curr.next`, `prev`, and `curr`) are constant time, or **O(1)**.

Since we perform an O(1) operation `n` times, the total time complexity is O(n).

---

### Space Complexity: O(1)

The space complexity is **O(1)** (constant space).

**Reasoning:**
The algorithm only uses a few extra variables to keep track of the nodes: `prev`, `curr`, and `temp`.

The amount of memory used for these pointers is *constant*. It does not matter if the list has 5 nodes or 5 million nodes; we are only ever using these three pointer variables. No other data structures are created.

---

### A Note on the Recursive Solution

As a point of comparison (and as mentioned in the problem's follow-up), the common recursive solution for this problem has a different space complexity:

* **Recursive Time:** **O(n)** (it still has to visit every node).
* **Recursive Space:** **O(n)** (because each recursive call adds a new frame to the call stack. The stack will grow as deep as the list itself).

This is why the iterative solution is often preferred, as it achieves the same result without using extra space on the call stack.

### The Logic Explained

We use two main pointers, `prev` and `curr`, to keep track of our position.

  * `prev`: This pointer tracks the node *behind* `curr`. It's initialized to `None` because the original `head` node will become the *last* node, and its `next` pointer should be `None`.
  * `curr`: This pointer tracks the *current* node we are processing. It starts at the `head` of the list.

Imagine the list is `1 -> 2 -> 3 -> None`.

1.  **Initial State:**

      * `prev = None`
      * `curr = 1`

2.  **Loop 1:**

      * `temp = curr.next` (Save the rest of the list: `temp` is now `2`)
      * `curr.next = prev` (Reverse the pointer: `1 -> None`)
      * `prev = curr` (Move `prev` up: `prev` is now `1`)
      * `curr = temp` (Move `curr` up: `curr` is now `2`)
      * **State:** `None <- 1` and we are looking at `2` (which still points to `3`).

3.  **Loop 2:**

      * `temp = curr.next` (Save the rest: `temp` is now `3`)
      * `curr.next = prev` (Reverse the pointer: `2 -> 1`)
      * `prev = curr` (Move `prev` up: `prev` is now `2`)
      * `curr = temp` (Move `curr` up: `curr` is now `3`)
      * **State:** `None <- 1 <- 2` and we are looking at `3`.

4.  **Loop 3:**

      * `temp = curr.next` (Save the rest: `temp` is now `None`)
      * `curr.next = prev` (Reverse the pointer: `3 -> 2`)
      * `prev = curr` (Move `prev` up: `prev` is now `3`)
      * `curr = temp` (Move `curr` up: `curr` is now `None`)
      * **State:** `None <- 1 <- 2 <- 3`.

5.  **End Loop:**

      * The `while curr:` loop terminates because `curr` is `None`.
      * The new head of the reversed list is the last node we processed, which is stored in `prev`.
      * We return `prev` (which points to `3`).

-----
