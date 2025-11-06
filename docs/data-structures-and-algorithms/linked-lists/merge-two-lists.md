---
title: Merge Two Sorted Linked Lists
layout: default
parent: Linked Lists
---

## Merge Two Sorted Linked Lists

You are given the heads of two sorted linked lists, `list1` and `list2`.

Merge the two lists into one **sorted** list. The list should be made by splicing together the nodes of the first two lists.

Return the head of the merged linked list.

---

### Examples

**Example 1:**
* **Input:** `list1 = [1,2,4]`, `list2 = [1,3,4]`
* **Output:** `[1,1,2,3,4,4]`



**Example 2:**
* **Input:** `list1 = []`, `list2 = []`
* **Output:** `[]`

**Example 3:**
* **Input:** `list1 = []`, `list2 = [0]`
* **Output:** `[0]`

---

### Constraints

* The number of nodes in both lists is in the range `[0, 50]`.
* `-100 <= Node.val <= 100`
* Both `list1` and `list2` are sorted in **non-decreasing order**.

By the way, to unlock the full functionality of all Apps, enable [Gemini Apps Activity](https://myactivity.google.com/product/gemini).

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next

class Solution:
    def mergeTwoLists(self, list1: Optional[ListNode], list2: Optional[ListNode]) -> Optional[ListNode]:
        dummy_node = node = ListNode()

        while list1 and list2:
            if list1.val < list2.val:
                node.next = list1
                list1 = list1.next
            else:
                node.next = list2
                list2 = list2.next
            node = node.next
                
        node.next = list1 or list2

        return dummy_node.next
```

Here's the time and space complexity for merging two sorted linked lists, letting **$m$** be the number of nodes in `list1` and **$n$** be the number of nodes in `list2`.

Both the iterative and recursive solutions have the same time complexity, but they differ in space complexity.

---

### ⏱️ Time Complexity: $O(m + n)$

* **Explanation:** In the worst case, you must visit every single node in both `list1` and `list2` exactly once to compare them and build the new merged list. The total number of operations is directly proportional to the total number of nodes ($m + n$).

---

### 💾 Space Complexity

This is where the two common approaches differ.

#### Iterative Solution: $O(1)$
* **Explanation:** The iterative (loop-based) solution only uses a few extra pointers (e.g., a `dummy` head and a `current` pointer) to build the new list. This is **constant space** because the amount of memory used does not change regardless of the size of the input lists. The merging is done "in-place" by rearranging the `next` pointers of the existing nodes.

#### Recursive Solution: $O(m + n)$
* **Explanation:** The recursive solution uses the **call stack** for its operations. Each recursive call adds a new frame to the stack. In the worst case, the recursion will go as deep as the total number of nodes ($m + n$), consuming stack space proportional to the total input size. For example, if the lists are interleaved (e.g., `[1, 3, 5]` and `[2, 4, 6]`), each call will make another call until both lists are empty.