---
title: Course Schedule
layout: home
parent: Graphs
---

```
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