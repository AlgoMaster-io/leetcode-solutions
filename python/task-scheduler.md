# [Leetcode 621: Task Scheduler](https://leetcode.com/problems/task-scheduler/)

## Approaches:

1. [Basic Approach: Greedy Simulation (Priority Queue)](#basic-approach-greedy-simulation-priority-queue)
2. [Optimal Approach: Count Analysis](#optimal-approach-count-analysis)

### Basic Approach: Greedy Simulation (Priority Queue)

**Intuition**:  
The task scheduler can be simulated using a greedy strategy that always selects the most frequent task that is eligible to be executed next. To facilitate this, a priority queue (max-heap) can be employed to dynamically fetch the most frequent task that can be executed without violating the cooldown period.

**Approach**:
- Use a max-heap (priority queue) to always execute the task with the highest remaining frequency.
- Use a cool-down mechanism (a queue) to keep track of tasks that are waiting for their cooldown to finish.
- Keep track of the time. At each time step, add tasks back to the max-heap if they have finished cooling down and execute a task from the max-heap if available.

**Steps**:
1. Count the frequency of each task using a dictionary and build a max-heap.
2. Use a queue to simulate the cooldown effect.
3. Iterate through each unit of time:
   - Add any tasks that have cooled down back into the max-heap.
   - Execute the task from the max-heap and move it into the cooldown queue with the updated frequency.
4. Continue until all tasks are scheduled.

```python
from collections import Counter, deque
import heapq

def leastInterval(tasks, n):
    # Create a frequency map of the tasks
    freq = Counter(tasks)
    
    # Create a max-heap based on task frequency
    max_heap = [-cnt for cnt in freq.values()]
    heapq.heapify(max_heap)
    
    # Queue to keep track of cooldown tasks
    cooldown_queue = deque()
    
    time = 0

    while max_heap or cooldown_queue:
        time += 1

        if max_heap:
            # Get the task with the highest frequency
            current = heapq.heappop(max_heap)
            current += 1  # Reduce its count

            if current != 0:
                # Add to cooldown queue with a cooldown time
                cooldown_queue.append((current, time + n))

        # Check if any tasks are cooled down and push them back into max-heap
        if cooldown_queue and cooldown_queue[0][1] == time:
            heapq.heappush(max_heap, cooldown_queue.popleft()[0])
    
    return time
```

**Time Complexity**: \(O(T \log T)\), where \(T\) is the total number of tasks, due to heap operations.
   
**Space Complexity**: \(O(T)\), for storing frequencies and the cooldown queue.

---

### Optimal Approach: Count Analysis

**Intuition**:  
This approach relies on analyzing the highest frequency of tasks to determine the minimum time required to complete all tasks while enforcing the cooldown period, aiming to avoid unnecessary idle time.

**Approach**:
- Calculate the maximum frequency of tasks.
- Determine the number of idle slots needed to fit the most frequent tasks.
- Calculate the shifting time by considering the number of slots not filled by other tasks.
- Optimize the order to minimize idle slots and determine the overall duration.

**Steps**:
1. Count the frequency of each task.
2. Determine the `max frequency` of any task.
3. Calculate potential idle slots using `max frequency`.
4. Fill these slots and calculate necessary intervals.
5. Calculate the final length by enforcing maximum between tasks and designed frame.

```python
from collections import Counter

def leastInterval(tasks, n):
    # Count frequencies of each task
    task_counts = Counter(tasks)
    max_freq = max(task_counts.values())
    
    # Determine the number of tasks that have maximum frequency
    tasks_with_max_freq = list(task_counts.values()).count(max_freq)
    
    # Calculate ideal frame size and total length
    intervals_needed = (max_freq - 1) * (n + 1) + tasks_with_max_freq
    
    return max(len(tasks), intervals_needed)
```

**Time Complexity**: \(O(T)\), given the direct operations on the tasks and their frequencies.
   
**Space Complexity**: \(O(1)\), as we only store a few additional variables for the computation.

