---
title: Data Structures and Algorithms
layout: home
---


When you see a new problem, you're not trying to remember if you've seen that *exact* problem. Instead, you're trying to identify *which pattern* it belongs to.

Here is a roadmap built around this "logic gate" concept, covering the patterns needed to solve the ~75 most common problems (like those found in the "Blind 75" or "NeetCode 75" lists).

---

## 🗺️ The DSA Roadmap: A Logic Gate Approach

### Phase 1: The Foundation (The Main Power)

Before you can use any logic gates, you need to understand the power source. In DSA, this is **Big O Notation**.

* **Logic Gate:** Before writing *any* code, ask:
    * "How much **time** will this take as the input grows?" (Time Complexity)
    * "How much **memory** (space) will this take?" (Space Complexity)
* **Goal:** Instantly know the time/space cost of basic operations:
    * Accessing an array element by index: $O(1)$
    * Searching an unsorted array: $O(n)$
    * Searching a sorted array (Binary Search): $O(\log n)$
    * Adding/Getting from a Hash Map: $O(1)$ on average
    * Sorting an array: $O(n \log n)$

---

### Phase 2: The Core Toolkit (The Components)

These are your basic data structures. Your first "logic gate" is choosing the right tool for the job.

| Problem Type (Input) | ➡️ | Logic Gate (Data Structure) | ⚡ Why? (Key Property) |
| :--- | :--- | :--- | :--- |
| I have a collection. I need to access items by **index**. | ➡️ | **Array / String** | $O(1)$ read access. |
| I need to store **key-value pairs**. I need *very fast* lookups. | ➡️ | **Hash Map** (or Set) | $O(1)$ average insert, delete, and search. |
| I need to process items **Last-In, First-Out (LIFO)**. | ➡️ | **Stack** | $O(1)$ push and pop. (Think: balanced parens). |
| I need to process items **First-In, First-Out (FIFO)**. | ➡️ | **Queue** | $O(1)$ add (enqueue) and remove (dequeue). |
| I need *fast* insertion/deletion, but *don't* need fast search. | ➡️ | **Linked List** | $O(1)$ insertion/deletion (if you have the node). |
| I need to find the **smallest or largest** item *repeatedly*. | ➡️ | **Heap (Priority Queue)** | $O(\log n)$ to insert or extract the min/max. |
| The problem involves **hierarchical data** (e.g., file system). | ➡️ | **Tree** (Binary Tree, BST) | $O(\log n)$ search/insert for a *balanced* BST. |
| The problem is about **string prefixes** or auto-complete. | ➡️ | **Trie (Prefix Tree)** | $O(L)$ search/insert, where $L$ is word length. |
| The problem describes a **network** or **connections**. | ➡️ | **Graph** | Represents nodes and edges (e.g., cities and roads). |

---

### Phase 3: The Common Patterns (The "Logic Circuits")

This is the core of your request. These are the patterns that solve the vast majority of the "Common 75" problems.

#### 1. The "Two Pointers" Pattern
* **Logic Gate:** If you have a **sorted array** (or a problem that can be simplified by sorting) and you're looking for a **pair** or **subsequence** that meets a condition.
* **Common Use:**
    * **Converging Pointers:** One pointer starts at the beginning, one at the end. They move toward each other.
    * **Fast/Slow Pointers:** Both start at the beginning, but one moves faster. (Common in Linked Lists to find cycles or middles).
* **Example Problems:**
    * Valid Palindrome
    * 3Sum
    * Container With Most Water
    * Linked List Cycle

#### 2. The "Sliding Window" Pattern
* **Logic Gate:** If the problem asks for the **longest**, **shortest**, or **best** *contiguous subarray (or substring)* that satisfies a condition.
* **Common Use:**
    * You have two pointers (a "window"). You *expand* the window by moving the *end* pointer.
    * When the window no-longer meets the condition, you *contract* it by moving the *start* pointer.
* **Example Problems:**
    * Best Time to Buy and Sell Stock
    * Longest Substring Without Repeating Characters
    * Minimum Window Substring

#### 3. The "Tree Traversal" Pattern (BFS & DFS)
* **Logic Gate (BFS):** If the problem asks to search a tree (or graph) **level by level** OR asks for the **shortest path** in an *unweighted* graph.
    * **Tool:** Use a **Queue**.
* **Logic Gate (DFS):** If the problem involves **exploring all paths**, checking for **connectivity**, or *validating* the structure of a tree.
    * **Tool:** Use **Recursion** (which uses the call *stack*) or an explicit **Stack**.
* **Example Problems:**
    * Maximum Depth of Binary Tree (DFS)
    * Binary Tree Level Order Traversal (BFS)
    * Validate Binary Search Tree (DFS)
    * Number of Islands (BFS or DFS)

#### 4. The "Top K Elements" Pattern
* **Logic Gate:** If the problem asks for the "**Kth largest**," "**Kth smallest**," or "**Top K**" frequent elements.
* **Common Use:**
    * Add items to a **Min-Heap** or **Max-Heap** of size $K$.
    * The heap will automatically manage the "Top K" items for you.
* **Example Problems:**
    * Kth Largest Element in an Array
    * Top K Frequent Elements

#### 5. The "Merge Intervals" Pattern
* **Logic Gate:** If the problem gives you a list of **intervals** `[start, end]` and asks you to find **overlaps** or merge them.
* **Common Use:**
    * **Sort the intervals** by their *start time*.
    * Iterate through the list, comparing the current interval's *end* with the next interval's *start*.
* **Example Problems:**
    * Merge Intervals
    * Non-overlapping Intervals

---

### Phase 4: The Advanced Patterns (The "CPU")

These patterns are more complex and often combine the building blocks from Phases 2 & 3.

#### 6. The "Dynamic Programming (DP)" Pattern
* **Logic Gate:** If you have an **optimization problem** (find the *max*, *min*, *longest*, *shortest*) that can be solved by breaking it into **smaller, overlapping subproblems**.
* **Common Use:**
    * **Memoization (Top-Down):** Use recursion, but store (memoize) the results of subproblems in a hash map or array so you don't calculate them twice.
    * **Tabulation (Bottom-Up):** Build a 1D or 2D array (a "DP table") from the simplest case up to your target answer.
* **Example Problems:**
    * Climbing Stairs
    * Coin Change
    * Longest Increasing Subsequence
    * Word Break

#### 7. The "Backtracking" Pattern
* **Logic Gate:** If the problem asks for **all possible solutions** (e.g., all *permutations*, *combinations*, or *subsets*) that meet a certain constraint.
* **Common Use:**
    * It's a "brute force" approach using **DFS (Recursion)**.
    * You make a *choice*, *explore* that path (recursive call), and then *un-choose* (backtrack) to try the next choice.
* **Example Problems:**
    * Subsets
    * Permutations
    * Combination Sum

#### 8. The "Advanced Graph" Pattern
* **Logic Gate:** If it's a graph problem, but more complex than simple traversal.
* **Common Use:**
    * **Shortest path in a *weighted* graph?** -> Use **Dijkstra's Algorithm** (which uses a Min-Heap).
    * **Any path from set A to set B?** -> Run BFS/DFS from *all* nodes in A (e.g., Pacific Atlantic Water Flow).
* **Example Problems:**
    * Course Schedule (Topological Sort)
    * Pacific Atlantic Water Flow

---

### 💡 How to Use This Roadmap

1.  **Read the Problem:** Don't code yet.
2.  **Identify Keywords:** Look for clues.
    * "shortest path," "level by level" -> **BFS**.
    * "contiguous subarray," "longest substring" -> **Sliding Window**.
    * "sorted array," "pair" -> **Two Pointers**.
    * "Top K," "Kth largest" -> **Heap**.
    * "all combinations," "all permutations" -> **Backtracking**.
    * "min/max ways to do," "overlapping subproblems" -> **Dynamic Programming**.
3.  **Select Your Pattern:** This is your primary "logic gate."
4.  **Select Your Tool:** Based on the pattern, pick your data structure (Hash Map, Queue, Heap, etc.).
5.  **Implement & Analyze:** Write the code, then double-check the **Big O** complexity. Ask yourself: "Can I optimize this?" (e.g., "This is $O(n^2)$, can I use a Hash Map to make it $O(n)$?")
