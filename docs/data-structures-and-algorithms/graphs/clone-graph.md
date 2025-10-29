---
title: Clone Graph
layout: home
parent: Graphs
---

## Clone Graph

Given a reference of a node in a **connected undirected graph**.

Return a **deep copy (clone)** of the graph.

Each node in the graph contains a value (`int`) and a list (`List[Node]`) of its neighbors.

```python
class Node {
    public int val;
    public List<Node> neighbors;
}
```

**Test case format:**

For simplicity, each node's value is the same as the node's index (1-indexed). For example, the first node with `val == 1`, the second node with `val == 2`, and so on. The graph is represented in the test case using an adjacency list.

An **adjacency list** is a collection of unordered lists used to represent a finite graph. Each list describes the set of neighbors of a node in the graph.

The given node will always be the first node with `val = 1`. You must return the copy of the given node as a reference to the cloned graph.

-----

### Example 1:

**Input:** `adjList = [[2,4],[1,3],[2,4],[1,3]]`
**Output:** `[[2,4],[1,3],[2,4],[1,3]]`
**Explanation:**
There are 4 nodes in the graph.
1st node (val = 1)'s neighbors are 2nd node (val = 2) and 4th node (val = 4).
2nd node (val = 2)'s neighbors are 1st node (val = 1) and 3rd node (val = 3).
3rd node (val = 3)'s neighbors are 2nd node (val = 2) and 4th node (val = 4).
4th node (val = 4)'s neighbors are 1st node (val = 1) and 3rd node (val = 3).

-----

### Example 2:

**Input:** `adjList = [[]]`
**Output:** `[[]]`
**Explanation:** Note that the input contains one empty list. The graph consists of only one node with val = 1 and it does not have any neighbors.

-----

### Example 3:

**Input:** `adjList = []`
**Output:** `[]`
**Explanation:** This is an empty graph, it does not have any nodes.

-----

### Constraints:

  * The number of nodes in the graph is in the range `[0, 100]`.
  * `1 <= Node.val <= 100`
  * `Node.val` is unique for each node.
  * There are no repeated edges and no self-loops in the graph.
  * The graph is connected and all nodes can be visited starting from the given node.
# Definition for a Node.

## Solution
```python
class Node:
    def __init__(self, val = 0, neighbors = None):
        self.val = val
        self.neighbors = neighbors if neighbors is not None else []

from typing import Optional

class Solution:
    def cloneGraph(self, node: Optional['Node']) -> Optional['Node']:
        old_to_new_map = {}

        def dfs_clone(original_node: 'Node') -> 'Node':
            if original_node in old_to_new_map:
                return old_to_new_map[original_node]
            cloned_node = Node(original_node.val)
            old_to_new_map[original_node] = cloned_node
            for neighbor in original_node.neighbors:
                cloned_node.neighbors.append(dfs_clone(neighbor))
            return cloned_node
 
        if not node:
            return None
            
        return dfs_clone(node)
```

## Code with Comments

```python
class Node:
    def __init__(self, val = 0, neighbors = None):
        self.val = val
        self.neighbors = neighbors if neighbors is not None else []

from typing import Optional

class Solution:
    def cloneGraph(self, node: Optional['Node']) -> Optional['Node']:
        """
        Clones an undirected graph.
        Args:
            node: The starting node of the graph to be cloned.
        Returns:
            The corresponding node in the cloned graph.
        """
        
        # A dictionary to map original nodes to their new, cloned counterparts.
        # This prevents re-cloning nodes and handles cycles.
        old_to_new_map = {}

        def dfs_clone(original_node: 'Node') -> 'Node':
            """
            Recursively clones the graph using Depth-First Search.
            Args:
                original_node: The current node from the original graph.
            Returns:
                The cloned copy of the node.
            """
            
            # If we've already cloned this node, return the existing copy.
            if original_node in old_to_new_map:
                return old_to_new_map[original_node]
            
            # 1. Create the new (cloned) node
            cloned_node = Node(original_node.val)
            
            # 2. Add the new node to the map *before* exploring neighbors.
            #    This is crucial for handling cycles.
            old_to_new_map[original_node] = cloned_node
            
            # 3. Recursively clone all neighbors and add them to the new node's list
            for neighbor in original_node.neighbors:
                cloned_node.neighbors.append(dfs_clone(neighbor))
                
            return cloned_node

        # Handle the edge case of an empty graph
        if not node:
            return None
            
        return dfs_clone(node)

Let $N$ be the number of nodes (vertices) and $E$ be the number of edges in the graph.
```
---

### ⏱️ Time Complexity: $O(N + E)$

The time complexity is $O(N + E)$ because the algorithm must visit every node and every edge exactly once.

1.  **Visiting Nodes ($O(N)$):** The `dfs_clone` function is called for each node. Thanks to the `old_to_new_map` (which acts as a 'visited' set), each node is processed (i.e., its copy is created) only one time. This accounts for the $O(N)$ part.
2.  **Visiting Edges ($O(E)$):** Inside the function, the `for` loop iterates through all the `neighbors` of the *current* node. Over the entire execution of the algorithm, this loop will run once for every single edge in the graph. This accounts for the $O(E)$ part.

Combining these, the total time is proportional to the number of nodes plus the number of edges.

---

### 💾 Space Complexity: $O(N)$

The space complexity is $O(N)$ due to the storage required for the recursion and the hash map.

1.  **Hash Map ($O(N)$):** The `old_to_new_map` dictionary stores a mapping for every node in the graph. In the worst case, it will hold $N$ entries (one for each node).
2.  **Recursion Stack ($O(N)$):** The space used by the call stack depends on the depth of the recursion. In the worst case, for a graph that is a long, unbranched chain (like a linked list), the recursion could go $N$ levels deep.

Since both factors are $O(N)$, the total auxiliary space complexity simplifies to $O(N)$.

> **Note:** This analysis doesn't include the space required for the *output* (the new, cloned graph itself), which would be $O(N + E)$. When discussing space complexity, we typically focus on the *auxiliary* space the algorithm uses during execution.


Here is an in-depth explanation of the "Clone Graph" solution.

### 1\. The Core Problem: Deep Copy vs. Shallow Copy

The problem asks for a **deep copy**, not a shallow copy.

  * A **shallow copy** would just create a new `Node` for the starting node (let's say `Node 1`) and then have its `neighbors` list point to the *original* `Node 2` and `Node 4`. This is useless, as we're still connected to the old graph.
  * A **deep copy** means we must create a *brand new, distinct* `Node` object for *every single node* in the original graph. The new `New Node 1` must have a `neighbors` list that points to the new `New Node 2` and the new `New Node 4`.

This task has two major challenges:

1.  **Cycles:** Graphs are not linear. A node can point to a node that points back to it (e.g., `Node 1` -\> `Node 2` -\> `Node 1`). If we just "copy and recurse" naively, we'll get an infinite loop and a stack overflow.
2.  **Shared Nodes:** A single node can be a neighbor to many other nodes (e.g., `Node 1` and `Node 3` both point to `Node 4`). We must ensure that our *cloned* `New Node 1` and *cloned* `New Node 3` point to the *exact same* `New Node 4` object. We cannot create two separate clones of `Node 4`.

### 2\. The Solution: Graph Traversal + Hash Map

The solution elegantly solves both problems using a **Hash Map** (a Python dictionary) combined with a **Graph Traversal** algorithm (in this case, Depth-First Search or DFS).

This map, which we called `old_to_new_map`, is the key to the entire algorithm.

`old_to_new_map = {}`

Its purpose is twofold:

1.  It acts as a **"visited" set**. If an *original* node is in the map's keys, it means we have *already* started processing (or finished processing) that node. This solves the **cycle problem**.
2.  It stores the **mapping** from an original node to its brand new clone. `map[original_node] = cloned_node`. This solves the **shared node problem**.

-----

### 3\. Code Analysis (Line-by-Line)

Let's break down the logic of the `dfs_clone` helper function, as it does all the work.

```python
def dfs_clone(original_node: 'Node') -> 'Node':
```

This function takes one argument: a node from the *original* graph. It promises to return the corresponding node from the *cloned* graph.

```python
    # If we've already cloned this node, return the existing copy.
    if original_node in old_to_new_map:
        return old_to_new_map[original_node]
```

This is the **most important check**. It solves both of our main problems at once.

  * **Cycle Handling:** If `Node 2` points back to `Node 1`, and we're already cloning `Node 1`, this check will find `Node 1` in the map and simply return its clone (`New Node 1`) instead of trying to re-clone it (which would cause an infinite loop).
  * **Shared Node Handling:** If we've already created `New Node 4` (because we got to it from `Node 1`), when we later get to it from `Node 3`, this check will find `Node 4` in the map and just return the *existing* `New Node 4`.

<!-- end list -->

```python
    # 1. Create the new (cloned) node
    cloned_node = Node(original_node.val)
```

If the node is *not* in the map, it's our first time seeing it. We create its clone. This new `cloned_node` has the correct `val`, but its `neighbors` list is currently empty.

```python
    # 2. Add the new node to the map *before* exploring neighbors.
    old_to_new_map[original_node] = cloned_node
```

This is the **second-most important step**. We *immediately* register the new clone in our map. We do this *before* we process its neighbors. This is crucial. It "locks in" the fact that `cloned_node` is the one true clone for `original_node`. If any future recursive call (even a deep one) circles back to `original_node`, it will find `cloned_node` in the map and stop.

```python
    # 3. Recursively clone all neighbors and add them to the new node's list
    for neighbor in original_node.neighbors:
        cloned_node.neighbors.append(dfs_clone(neighbor))
```

This is the recursive step. We iterate through all the *original* node's neighbors. For each neighbor, we make a recursive call to `dfs_clone`.

That call will do one of two things:

1.  If that `neighbor` hasn't been cloned yet, it will create a new clone, add it to the map, and return it.
2.  If that `neighbor` *has* been cloned (or is in the process of being cloned), it will hit the `if` statement at the top and immediately return the *existing* clone from the map.

Either way, we `append` the *correct, unique clone* of that neighbor to our `cloned_node`'s `neighbors` list.

```python
    return cloned_node
```

After the loop finishes, our `cloned_node` is now fully constructed. Its `val` is correct, and its `neighbors` list is populated with all the *correct cloned neighbors*. We return it up the call stack.

-----

### 4\. Step-by-Step Example Trace

Let's trace `adjList = [[2,4],[1,3],[2,4],[1,3]]`.

  * `Call 1: cloneGraph(Node 1)`

      * `old_to_new_map` is `{}`.
      * Calls `dfs_clone(Node 1)`.

  * `Call 2: dfs_clone(Node 1)`

      * `Node 1` is not in map.
      * Create `New Node 1`.
      * `old_to_new_map` is now `{ Node 1: New Node 1 }`.
      * Loop `Node 1`'s neighbors: `[Node 2, Node 4]`.
      * First neighbor is `Node 2`.
      * Calls `dfs_clone(Node 2)`.

  * `Call 3: dfs_clone(Node 2)`

      * `Node 2` is not in map.
      * Create `New Node 2`.
      * `old_to_new_map` is now `{ Node 1: New Node 1, Node 2: New Node 2 }`.
      * Loop `Node 2`'s neighbors: `[Node 1, Node 3]`.
      * First neighbor is `Node 1`.
      * Calls `dfs_clone(Node 1)`.

  * `Call 4: dfs_clone(Node 1)`

      * `Node 1` **IS IN THE MAP\!**
      * Returns `old_to_new_map[Node 1]`, which is `New Node 1`.

  * (Back in `Call 3: dfs_clone(Node 2)`)

      * `New Node 2.neighbors.append(New Node 1)`.
      * Second neighbor is `Node 3`.
      * Calls `dfs_clone(Node 3)`.

  * `Call 5: dfs_clone(Node 3)`

      * `Node 3` is not in map.
      * Create `New Node 3`.
      * `old_to_new_map` is now `{ Node 1: N1, Node 2: N2, Node 3: N3 }`.
      * Loop `Node 3`'s neighbors: `[Node 2, Node 4]`.
      * First neighbor is `Node 2`.
      * Calls `dfs_clone(Node 2)`.

  * `Call 6: dfs_clone(Node 2)`

      * `Node 2` **IS IN THE MAP\!**
      * Returns `old_to_new_map[Node 2]`, which is `New Node 2`.

  * (Back in `Call 5: dfs_clone(Node 3)`)

      * `New Node 3.neighbors.append(New Node 2)`.
      * Second neighbor is `Node 4`.
      * Calls `dfs_clone(Node 4)`.

  * `Call 7: dfs_clone(Node 4)`

      * `Node 4` is not in map.
      * Create `New Node 4`.
      * `old_to_new_map` is now `{ Node 1: N1, Node 2: N2, Node 3: N3, Node 4: N4 }`.
      * Loop `Node 4`'s neighbors: `[Node 1, Node 3]`.
      * First neighbor is `Node 1`. Calls `dfs_clone(Node 1)`. This returns `New Node 1` from the map.
      * `New Node 4.neighbors.append(New Node 1)`.
      * Second neighbor is `Node 3`. Calls `dfs_clone(Node 3)`. This returns `New Node 3` from the map.
      * `New Node 4.neighbors.append(New Node 3)`.
      * Loop finishes. Returns `New Node 4`.

  * (Back in `Call 5: dfs_clone(Node 3)`)

      * `New Node 3.neighbors.append(New Node 4)`.
      * Loop finishes. Returns `New Node 3`.

  * (Back in `Call 3: dfs_clone(Node 2)`)

      * `New Node 2.neighbors.append(New Node 3)`.
      * Loop finishes. Returns `New Node 2`.

  * (Back in `Call 2: dfs_clone(Node 1)`)

      * `New Node 1.neighbors.append(New Node 2)`.
      * Second neighbor is `Node 4`.
      * Calls `dfs_clone(Node 4)`.

  * `Call 8: dfs_clone(Node 4)`

      * `Node 4` **IS IN THE MAP\!**
      * Returns `old_to_new_map[Node 4]`, which is `New Node 4`.

  * (Back in `Call 2: dfs_clone(Node 1)`)

      * `New Node 1.neighbors.append(New Node 4)`.
      * Loop finishes. Returns `New Node 1`.

  * (Back in `Call 1: cloneGraph`)

      * The final result, `New Node 1`, is returned.

This process has successfully built a new graph where all `New Node` objects are distinct and correctly interconnected, perfectly mirroring the original graph's structure.