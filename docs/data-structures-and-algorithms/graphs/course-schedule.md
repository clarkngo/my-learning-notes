---
title: Course Schedule
layout: home
parent: Graphs
---

### 🎯 Problem Description

There are a total of **`numCourses`** courses you have to take, labeled from **`0`** to **`numCourses - 1`**.

You are given an array `prerequisites` where `prerequisites[i] = [ai, bi]` indicates that you must take course **`bi`** first if you want to take course **`ai`**.

* For example, the pair `[0, 1]` indicates that to take course `0` you have to first take course `1`.

Return **`true`** if you can finish all courses. Otherwise, return **`false`**.

---

### 📋 Examples

**Example 1:**

* **Input:** `numCourses = 2`, `prerequisites = [[1,0]]`
* **Output:** `true`
* **Explanation:** There are a total of 2 courses to take. To take course 1 you should have finished course 0. So it is possible.

**Example 2:**

* **Input:** `numCourses = 2`, `prerequisites = [[1,0],[0,1]]`
* **Output:** `false`
* **Explanation:** There are a total of 2 courses to take. To take course 1 you should have finished course 0, and to take course 0 you should also have finished course 1. So it is impossible.

---

### ⛓️ Constraints

* `1 <= numCourses <= 2000`
* `0 <= prerequisites.length <= 5000`
* `prerequisites[i].length == 2`
* `0 <= ai, bi < numCourses`
* All the pairs `prerequisites[i]` are **unique**.

## Solution

```python
class Solution:
    def canFinish(self, numCourses: int, prerequisites: List[List[int]]) -> bool:
        course_to_prereqs = {i: [] for i in range(numCourses)}

        for course, prereq in prerequisites:
            course_to_prereqs[course].append(prereq)

        visiting_in_this_path = set()

        def path_is_safe(course):
            if course in visiting_in_this_path:
                return False  
            if course_to_prereqs[course] == []:
                return True
                
            visiting_in_this_path.add(course)

            for prereq_course in course_to_prereqs[course]:
                if not path_is_safe(prereq_course):
                    return False
            visiting_in_this_path.remove(course)
            course_to_prereqs[course] = []
            
            return True 
            
        for course_num in range(numCourses):
            if not path_is_safe(course_num):
                return False
                
        return True
```

## Code with Comments

```python
class Solution:
    def canFinish(self, numCourses: int, prerequisites: List[List[int]]) -> bool:
        
        # 1. Build the Prerequisite Map
        # We create a map where each course (key) maps to a list of
        # courses that are its direct prerequisites (values).
        course_to_prereqs = {i: [] for i in range(numCourses)}
        for course, prereq in prerequisites:
            course_to_prereqs[course].append(prereq)
        
        # 2. Path Tracker (The "visited" set)
        # This is the most important part. This set does *not* track all
        # courses we've ever seen. It only tracks the courses we are
        # *currently visiting in this single recursive path*.
        # If we hit a course that's already in this set, we've found a cycle.
        visiting_in_this_path = set()

        # 3. The DFS (Depth-First Search) Function
        # We rename 'dfs' to 'path_is_safe'. This function checks:
        # "Starting from this 'course', is its entire prerequisite chain safe (free of cycles)?"
        def path_is_safe(course):
            
            # --- Base Case 1: CYCLE DETECTED ---
            # If we are trying to check a 'course' that is *already* in our
            # 'visiting_in_this_path' set, it means we've looped back to it.
            # Example: A -> B -> C -> A
            if course in visiting_in_this_path:
                return False  # Found a cycle!
            
            # --- Base Case 2: ALREADY PROVEN SAFE ---
            # If this 'course' has no prerequisites, it's safe by default.
            # (This also works as an optimization: once we prove a course
            # is safe, we clear its list at the end).
            if course_to_prereqs[course] == []:
                return True
            
            # --- Mark as "Currently Visiting" ---
            # Add this course to our current path tracker *before*
            # checking its prerequisites.
            visiting_in_this_path.add(course)
            
            # --- Recursively Check Prerequisites ---
            # Check every prerequisite for the current course.
            for prereq_course in course_to_prereqs[course]:
                # If *any* of the prerequisite paths are *not* safe (they found a cycle),
                # then this current 'course' is also unsafe.
                if not path_is_safe(prereq_course):
                    return False
            
            # --- Backtrack and Optimize ---
            # If we're here, it means all prerequisite paths for 'course'
            # were safe. This 'course' is officially safe.
            
            # 1. (Backtrack): Remove it from the *current* path tracker
            #    so that *other* paths can use it as a valid prerequisite.
            visiting_in_this_path.remove(course)
            
            # 2. (Optimize): We've proven 'course' is safe. We can clear
            #    its prerequisite list. Any future check for this 'course'
            #    will hit Base Case 2 and return True immediately.
            course_to_prereqs[course] = []
            
            return True  # This path is safe!

        # 4. Check Every Single Course
        # We must loop through all courses from 0 to numCourses-1.
        # This is because the prerequisite map might have "disconnected"
        # groups of courses (e.g., 1->2 and a separate 3->4->3 cycle).
        # We have to check every single one as a potential starting point.
        for course_num in range(numCourses):
            if not path_is_safe(course_num):
                # If checking *any* course reveals a cycle, we're done.
                return False
                
        # If we checked all courses and found no cycles, we can finish.
        return True
```

Here's the breakdown of the time and space complexity.

Let:

  * **$N$** = `numCourses` (the number of **nodes** in the graph)
  * **$P$** = `len(prerequisites)` (the number of **edges** in the graph)

-----

### ⏳ Time Complexity: $O(N + P)$

This is the standard time complexity for a **Depth-First Search (DFS)** on a graph. Here's why:

1.  **Building the Map ($O(P)$):**

      * The code first iterates through the entire `prerequisites` list, which has $P$ items.
      * For each item, it performs an $O(1)$ dictionary lookup and list append.
      * **Cost: $O(P)$**

2.  **The DFS (`path_is_safe`) ($O(N + P)$):**

      * This is the main part. The code has a `for` loop that calls `path_is_safe` for every course from `0` to $N-1$.
      * This *looks* like it might be $O(N \times \text{something})$, but it's not.
      * The **key optimization** is this part:
        ```python
        # (Optimize): We've proven 'course' is safe.
        course_to_prereqs[course] = [] 
        ```
      * Because of this line, the full `path_is_safe` function (with all its recursive checks) will run **at most once** for any given course.
      * If `path_is_safe(C)` is called again as a prerequisite for another course (e.g., `path_is_safe(A)` checks `B`, which checks `C`), it will immediately hit the `if course_to_prereqs[course] == []:` base case and return `True` in $O(1)$ time.
      * This means that across the *entire* execution, each node (course) and each edge (prerequisite) is visited **exactly one time**.
      * Visiting every node is $O(N)$.
      * Visiting every edge is $O(P)$.
      * **Cost: $O(N + P)$**

**Total Time:** $O(P) \text{ (map)} + O(N + P) \text{ (DFS)} = O(N + P)$

-----

### 💾 Space Complexity: $O(N + P)$

The space is determined by the data structures we build and the recursion call stack.

1.  **The Prerequisite Map ($O(N + P)$):**

      * `course_to_prereqs` stores one key for every course ($N$).
      * It also stores a value for every prerequisite ($P$) distributed across the lists.
      * **Cost: $O(N + P)$**

2.  **The Recursion Call Stack ($O(N)$):**

      * The `path_is_safe` function calls itself. In the worst-case scenario, the prerequisites form one long chain (e.g., $Course 0 \rightarrow 1 \rightarrow 2 \rightarrow ... \rightarrow N-1$).
      * This would cause the recursion to go $N$ levels deep, placing $N$ function calls on the stack.
      * **Cost: $O(N)$**

3.  **The `visiting_in_this_path` Set ($O(N)$):**

      * This set stores the nodes in the *current* path.
      * Its size is directly limited by the recursion depth. In that same worst-case long chain, the set would grow to hold $N$ items before backtracking.
      * **Cost: $O(N)$**

**Total Space:** $O(N + P) \text{ (map)} + O(N) \text{ (stack)} + O(N) \text{ (set)} = O(N + P)$

This solution uses a **Depth-First Search (DFS)** algorithm to solve the problem.

The entire problem boils down to one question: **"Is there a cycle in the course dependencies?"**

  * If there is **no cycle** (like `Course A` needs `B`, and `B` needs `C`), you **can** finish.
  * If there is a **cycle** (like `A` needs `B`, and `B` needs `A`), you **cannot** finish.

This code is a "cycle detector" for a directed graph.

-----

### 1\. The Core Idea: The Task List

Imagine you have a main task, like "Take Course 0".

1.  You look at your `course_to_prereqs` map: `0: [1, 2]`.
2.  To do Task 0, you must first do "Task 1" and "Task 2".
3.  You pause Task 0 and start "Task 1".
4.  You look at its list: `1: [3]`.
5.  You pause Task 1 and start "Task 3".
6.  You look at its list: `3: []`. It has no prerequisites. Task 3 is "safe." You complete it.
7.  You go back to Task 1. Since Task 3 is done, Task 1 is also "safe."
8.  You go back to Task 0. You still need to do "Task 2"... and so on.

**When does this fail?**
Imagine you're doing "Task A". It needs "Task B". You start "Task B". It needs "Task C". You start "Task C". It needs "Task A".

You are now trying to start "Task A", but you're **already in the middle of doing "Task A"** (you paused it to do B and C). This is a cycle, and it's impossible.

-----

### 2\. How the Code Implements This

#### **Step 1: Build the Map**

```python
course_to_prereqs = {i: [] for i in range(numCourses)}
for course, prereq in prerequisites:
    course_to_prereqs[course].append(prereq)
```

This just builds the "task list." It creates a map where each course points to its list of immediate prerequisites.

#### **Step 2: The "Current Path" Tracker**

```python
visiting_in_this_path = set()
```

This is the **most important part**. This set tracks the "tasks" (courses) we are *currently* in the middle of. In our analogy, it's the list of paused tasks: `{"Task A", "Task B", "Task C"}`.

It is **NOT** a set of all courses we've ever visited. It's *only* the ones in the current recursive chain.

#### **Step 3: The `path_is_safe(course)` Function**

This function (which was `dfs`) answers the question: "Can I complete this `course` and all its prerequisites without getting stuck in a loop?"

Let's trace its logic:

1.  ```python
    if course in visiting_in_this_path:
        return False  # CYCLE!
    ```

      * **Translation:** "I'm being asked to start this `course`, but I'm *already* in the middle of processing it." This is the cycle (like `A -> B -> A`). It's impossible.

2.  ```python
    if course_to_prereqs[course] == []:
        return True  # SAFE!
    ```

      * **Translation:** "This `course` has no prerequisites." It's a "base case" course (like "Course 101"). It's safe. (This also acts as an optimization, which we'll see in a second).

3.  ```python
    visiting_in_this_path.add(course)
    ```

      * **Translation:** "Okay, this course isn't a cycle *yet* and isn't a base case. Let's start processing it. Add it to our 'currently visiting' list."

4.  ```python
    for prereq_course in course_to_prereqs[course]:
        if not path_is_safe(prereq_course):
            return False
    ```

      * **Translation:** "Go through every prerequisite for this `course`. Recursively check if *that* path is safe. If any of my prerequisites (`prereq_course`) reports back `False` (meaning *it* found a cycle), then I'm also impossible. Stop and report `False`."

5.  ```python
    visiting_in_this_path.remove(course)
    course_to_prereqs[course] = []
    return True
    ```

      * **Translation:** "If my `for` loop finished, it means all my prerequisites (and *their* prerequisites, etc.) are safe. This `course` is officially 100% possible to take."
      * **`visiting_in_this_path.remove(course)`:** (This is "backtracking"). We are *done* checking this path. We remove it from the "currently visiting" set so that *other* courses can use it as a prerequisite without causing a false alarm.
      * **`course_to_prereqs[course] = []`:** (This is an **optimization**). We've proven this `course` is safe. We clear its prerequisite list. Now, if any *other* course check needs this `course`, it will hit Step 2 (`if course_to_prereqs[course] == []`) and immediately return `True` without re-doing all the work.

#### **Step 4: The Main Loop**

```python
for course_num in range(numCourses):
    if not path_is_safe(course_num):
        return False
return True
```

You might wonder, "Why check *every* course?"

This is to catch **disconnected graphs**. You might have one set of safe courses (like `0 -> 1 -> 2`) and a completely separate set of *unsafe* courses (`3 -> 4 -> 5 -> 3`).

If you only checked `path_is_safe(0)`, you'd get `True` and miss the cycle in the other group. This loop ensures you check every single course as a potential starting point for a cycle. If any of them find a cycle, the whole system is impossible.