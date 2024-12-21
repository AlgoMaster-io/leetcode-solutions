# [LeetCode 752: Open the Lock](https://leetcode.com/problems/open-the-lock/)

## Table of Contents
- [Approach 1: Breadth-First Search (BFS)](#approach-1-breadth-first-search-bfs)
- [Approach 2: Bidirectional Breadth-First Search (BiBFS)](#approach-2-bidirectional-breadth-first-search-bibfs)

---

## Approach 1: Breadth-First Search (BFS)

### Intuition
In this approach, we use the Breadth-First Search (BFS) technique to systematically explore all the possible states of the lock. BFS is suitable here because it explores all shortest possible paths first, ensuring that we find the minimum number of moves required to unlock the lock.

The idea is to start with the initial state "0000" and attempt to reach the target lock combination while avoiding "deadends". Each position on the lock can be turned up or down, generating neighbors for the state. By using a queue, we explore neighbors level by level, ensuring minimal moves first.

### Steps
1. Use a queue to keep track of the current lock combination and the number of moves.
2. Use a set to track visited combinations to prevent loops and redundant checks.
3. Iterate while the queue is not empty:
   - Dequeue the front element.
   - If it's the target, return the number of steps.
   - If it's in deadends, continue to the next iteration.
   - For each position in the lock, generate neighbors by turning each wheel forward or backward and add them to the queue if they haven't been visited.
4. If the queue is exhausted without finding the target, return -1.

### Code
```python
from collections import deque

def openLock(deadends, target):
    # Initial configuration
    if target == "0000":
        return 0
    visited = set(deadends)
    if "0000" in visited:
        return -1
    
    # Use deque for efficient popleft operation
    queue = deque([("0000", 0)])
    
    while queue:
        state, steps = queue.popleft()
        
        # If we reach the target state
        if state == target:
            return steps
        
        # Trying all possibilities by turning a wheel up or down
        for i in range(4):
            digit = int(state[i])
            for move in [-1, 1]:  # Wheel can be moved to -1 or +1
                new_digit = (digit + move) % 10
                new_state = state[:i] + str(new_digit) + state[i+1:]
                
                if new_state not in visited:
                    visited.add(new_state)
                    queue.append((new_state, steps + 1))
    
    return -1

# Complexities: 
# Time: O(10^4) - worst case explore all combinations
# Space: O(10^4) - worst case store all visited states
```

---

## Approach 2: Bidirectional Breadth-First Search (BiBFS)

### Intuition
Bidirectional Breadth-First Search aims to optimize the BFS approach by simultaneously performing BFS from both the initial state "0000" and the target state. By progressing from both ends, it can potentially reduce the search space and thereby optimize the solution.

Bidirectional BFS is impactful when most nodes are symmetrically reachable from both sides of the problem domain, as is the case with this lock mechanism.

### Steps
1. Initialize two queues, one from "0000" and another from `target`.
2. Use two visited sets to track the states seen from both the start and the target.
3. Perform alternate BFS step expansions from both queues:
   - At each step, check if an intersection is found in the visited nodes from both sides.
   - Generate neighbors as in the unidirectional BFS.
   - If the intersection occurs between two sets, calculate and return the total moves.
4. If both queues are exhausted without meeting an intersection point, return -1.

### Code
```python
from collections import deque

def openLock(deadends, target):
    if target == "0000":
        return 0
    deadset = set(deadends)
    if "0000" in deadset:
        return -1
    
    # Initialize two sets of visited states and queues
    forward_queue = deque([("0000", 0)])
    backward_queue = deque([(target, 0)])
    
    forward_visited = {"0000"}
    backward_visited = {target}
    
    def bfs_step(queue, visited, other_visited):
        state, steps = queue.popleft()
        
        for i in range(4):
            digit = int(state[i])
            for move in [-1, 1]:
                new_digit = (digit + move) % 10
                new_state = state[:i] + str(new_digit) + state[i+1:]
                
                if new_state in other_visited:
                    return steps + 1
                
                if new_state not in visited and new_state not in deadset:
                    visited.add(new_state)
                    queue.append((new_state, steps + 1))
        
        return None
    
    while forward_queue and backward_queue:
        # Expand from the forward side
        result = bfs_step(forward_queue, forward_visited, backward_visited)
        if result is not None:
            return result
        
        # Expand from the backward side
        result = bfs_step(backward_queue, backward_visited, forward_visited)
        if result is not None:
            return result
    
    return -1

# Complexities:
# Time: O(10^4) - theoretically similar to BFS for meeting in the middle
# Space: O(10^4) - worst case store states from both sides
```

Bidirectional BFS can potentially lower the average-case runtime by meeting the solution in the middle. However, in practice, the original BFS is usually effective and simpler to understand due to the symmetry and constraints of the problem.

