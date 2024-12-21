# [1834. Single-Threaded CPU](https://leetcode.com/problems/single-threaded-cpu/)

## Approaches
- [Approach 1: Sort and Iterate with Priority Queue (Heap)](#approach-1-sort-and-iterate-with-priority-queue-heap)

---

## Approach 1: Sort and Iterate with Priority Queue (Heap)

The problem of scheduling tasks on a single-threaded CPU can be optimally solved using a combination of sorting and a priority queue (heap). The key is to manage when tasks become available and use the CPU efficiently.

### Intuition

1. **Sort Tasks**: 
   - First, sort the tasks by their enqueue time to simulate the timeline of when tasks become available. This helps us in processing tasks in the order they appear.

2. **Use a Priority Queue**: 
   - Utilize a min-heap to keep track of available tasks and choose the task with the smallest processing time, ensuring that the CPU is always executing the shortest available task first.

3. **Simulate the CPU Processing**:
   - Iterate over the sorted tasks and use a simulated time variable to determine when tasks should start and finish.
   - While processing tasks, check the priority queue for tasks that can now be executed and pick out the task with the minimum processing time.

### Detailed Steps

1. **Pair Tasks with Original Indices**: To maintain information about the original order for output, annotate tasks with their indices before sorting.
2. **Sort by Enqueue Time**: Sort tasks based on the enqueue time to know when tasks arrive.
3. **Iterate and Process**: 
   - Initialize a priority queue (min-heap) which sorts first by processing time and then by the original index for tie-breaking.
   - Initialize a pointer to track the current task, and a time variable to manage CPU scheduling.
   - Push tasks to the heap as their enqueue times come into the simulated time scope.
4. **Execute Tasks**: Continuously pop from the heap and process the task (increment current time by processing time).
5. **Move Pointer**: If the priority queue is empty and there are tasks remaining, move the pointer to the next available task.

### Implementation

```javascript
class MinHeap {
    constructor(comparator = (a, b) => a - b) {
        this.heap = [];
        this.comparator = comparator;
    }
    
    size() {
        return this.heap.length;
    }
    
    peek() {
        return this.heap[0];
    }
    
    push(value) {
        this.heap.push(value);
        this.bubbleUp();
    }
    
    pop() {
        const poppedValue = this.heap[0];
        const bottom = this.size() - 1;
        if (bottom > 0) {
            this.swap(0, bottom);
        }
        this.heap.pop();
        this.bubbleDown();
        return poppedValue;
    }
    
    bubbleUp() {
        let index = this.size() - 1;
        const element = this.heap[index];
        while (index > 0) {
            const parentIndex = Math.floor((index - 1) / 2);
            const parent = this.heap[parentIndex];
            if (this.comparator(element, parent) >= 0) break;
            this.swap(index, parentIndex);
            index = parentIndex;
        }
    }
    
    bubbleDown() {
        let index = 0;
        const length = this.size();
        const element = this.heap[0];
        while (true) {
            const leftChildIndex = 2 * index + 1;
            const rightChildIndex = 2 * index + 2;
            let leftChild, rightChild;
            let swapIndex = null;

            if (leftChildIndex < length) {
                leftChild = this.heap[leftChildIndex];
                if (this.comparator(leftChild, element) < 0) {
                    swapIndex = leftChildIndex;
                }
            }

            if (rightChildIndex < length) {
                rightChild = this.heap[rightChildIndex];
                if (
                    (swapIndex === null && this.comparator(rightChild, element) < 0) ||
                    (swapIndex !== null && this.comparator(rightChild, leftChild) < 0)
                ) {
                    swapIndex = rightChildIndex;
                }
            }

            if (swapIndex === null) break;
            this.swap(index, swapIndex);
            index = swapIndex;
        }
    }

    swap(i, j) {
        [this.heap[i], this.heap[j]] = [this.heap[j], this.heap[i]];
    }
}

function getOrder(tasks) {
    // Annotate tasks with original indices for later use
    tasks = tasks.map((task, index) => [...task, index]);

    // Sort the tasks based on their enqueue times
    tasks.sort((a, b) => a[0] - b[0]);

    const minHeap = new MinHeap((a, b) => {
      // First compare by processing time, then by the original index
      if (a[0] === b[0]) return a[1] - b[1];
      return a[0] - b[0];
    });

    const result = [];
    let currentTime = 0;
    let i = 0;
    const n = tasks.length;

    // Process tasks
    while (i < n || minHeap.size() > 0) {
        // Add all tasks that have become available by currentTime to the heap
        while (i < n && tasks[i][0] <= currentTime) {
            minHeap.push([tasks[i][1], tasks[i][2]]);
            i++;
        }

        if (minHeap.size() > 0) {
            const [processingTime, originalIndex] = minHeap.pop();
            result.push(originalIndex);
            currentTime += processingTime;
        } else {
            // If no tasks are available, jump to the next task's enqueue time
            currentTime = tasks[i][0];
        }
    }

    return result;
}
```

### Time Complexity

- Sorting the tasks takes \(O(n \log n)\).
- The priority queue operations for each task are \(O(\log n)\), thus for all tasks, it is \(O(n \log n)\).

### Space Complexity

- The priority queue holds all tasks in the worst case: \(O(n)\).
- Sorting requires additional space: \(O(n)\).
- Hence, overall space complexity is \(O(n)\).
  
This approach ensures that we utilize the CPU optimally by always choosing the shortest available task to execute next, minimizing the idle time and handling ties efficiently with help of task indices.

