# [LeetCode 502: IPO](https://leetcode.com/problems/ipo/)

## Table of Contents
- [Approach 1: Brute Force](#approach-1)
- [Approach 2: Priority Queue (Optimal Solution)](#approach-2)

---

## Approach 1: Brute Force

### Intuition
The brute force approach is to simulate the sequence of events when selecting projects. For each given capital, we will find the set of projects that can be undertaken given the current capital. From this set, we select the project with the maximum profit and update the capital. This is repeated `k` times or until no more projects can be undertaken.

### Steps
1. Start with the initial capital `w`.
2. Iterate `k` times (or until no projects can be picked).
3. In each iteration, select the project with the maximum profit that we can afford given the current capital.
4. Add the profit of the selected project to the current capital.
5. Repeat until `k` projects are selected or no more projects can be chosen.

### Code
```python
def findMaximizedCapital(k, w, profits, capital):
    n = len(profits)
    projects = list(zip(capital, profits))
    projects.sort()  # Sort projects by required capital

    for _ in range(k):
        # Find the project with the maximum profit we can afford at this moment
        max_profit = -1
        selected_index = -1
        for i in range(n):
            if projects[i][0] <= w:  # If we can afford this project
                if projects[i][1] > max_profit:  # Check if profit is greater than current max profit
                    max_profit = projects[i][1]
                    selected_index = i
        if selected_index == -1:  # No projects can be selected
            break
        # Update capital
        w += projects[selected_index][1]
        # Remove the selected project from the list or mark as used
        projects[selected_index] = (float('inf'), float('-inf'))

    return w
```

### Complexity Analysis
- **Time Complexity**: O(k * n) in the worst case because for each iteration we may have to scan all projects.
- **Space Complexity**: O(n) to store project tuples.

---

## Approach 2: Priority Queue (Optimal Solution)

### Intuition
To optimize the greedy selection of the most profitable project available, we use a max-heap (priority queue). The main idea is to iterate over projects, adding all projects that can be afforded with the current capital into the heap. After that, we pop the project with the highest profit from the heap, add its profit to the current capital, and repeat. This makes the selection process more efficient, avoiding the need to scan all projects during each selection.

### Steps
1. Create a list of tuples containing capital and profits.
2. Sort the list by capital in ascending order.
3. Use a max-heap to store available projects based on current capital.
4. For each iteration up to `k` or until no more projects can be afforded:
   - Push all projects from the list whose capital is less than or equal to the current capital into the max-heap.
   - Pop the max profit project from the heap and update the current capital.
   - Continue until `k` projects are completed or no more projects can be taken.

### Code
```python
import heapq

def findMaximizedCapital(k, w, profits, capital):
    n = len(capital)
    projects = list(zip(capital, profits))
    projects.sort()  # Sort projects by required capital i.e., the first element of tuples
    max_heap = []
    index = 0

    for _ in range(k):
        # Adding all projects that can be started with current capital into the max-heap
        while index < n and projects[index][0] <= w:
            heapq.heappush(max_heap, -projects[index][1])  # Use negative profits for max-heap
            index += 1
        if not max_heap:  # No projects are affordable
            break
        # Select the project with the maximum available profit
        w += -heapq.heappop(max_heap)  # Negate to get the actual profit

    return w
```

### Complexity Analysis
- **Time Complexity**: O(n log n) for sorting the projects + O(k log n) for heap operations, overall it is O((n + k) log n). We only iterate over projects once, and each heap operation is log n.
- **Space Complexity**: O(n) to maintain the heap and project tuples.

This approach efficiently selects the most profitable projects using a heap, allowing us to maximize capital in an optimal time.

