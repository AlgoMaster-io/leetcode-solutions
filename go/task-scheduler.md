# [Leetcode 621: Task Scheduler](https://leetcode.com/problems/task-scheduler/)

## Approaches:
- [Approach 1: Brute Force Simulation](#approach-1-brute-force-simulation)
- [Approach 2: Greedy with Priority Queue](#approach-2-greedy-with-priority-queue)
- [Approach 3: Greedy with Math Optimization](#approach-3-greedy-with-math-optimization)

## Approach 1: Brute Force Simulation

**Intuition:**  
In this approach, we simply simulate the scheduling task by dynamically adjusting the sequence of tasks we execute. We start executing tasks with the highest frequency and maintain a sequence that respects the cooling period constraint.

**Steps:**  
1. Count the frequency of each task using a map.
2. Loop over the time intervals, selecting tasks with the highest frequency that can be scheduled considering the cooling period.
3. Continue this process until all tasks are scheduled.

**Code:**
```go
func leastInterval(tasks []byte, n int) int {
    // Step 1: Count the frequency of each task.
    freq := make(map[byte]int)
    for _, task := range tasks {
        freq[task]++
    }

    // Variable to count time intervals
    intervals := 0

    // Temporary list to handle cooldown period assignments
    temp := make([]byte, 0)

    // Step 2: Prioritize high frequency tasks while maintaining cooling constraint
    for len(freq) > 0 {
        temp = make([]byte, 0)
        for i := 0; i <= n; i++ {
            selectedTask := byte(0)
            for task, count := range freq {
                // Pick a task with the highest frequency
                if selectedTask == 0 || freq[selectedTask] < count {
                    selectedTask = task
                }
            }
            
            if selectedTask != 0 {
                temp = append(temp, selectedTask)
                freq[selectedTask]--
                if freq[selectedTask] == 0 {
                    delete(freq, selectedTask)
                }
            }
            intervals++

            // If no tasks left to schedule and a cooling period had more slots than tasks
            if len(freq) == 0 && len(temp) <= i+1 {
                break
            }
        }
    }
    return intervals
}
```
**Complexity Analysis:**
- Time Complexity: O(N^2), where N is the number of tasks (querying the map for each cooling period).
- Space Complexity: O(1), only a fixed number of additional variables are used.

## Approach 2: Greedy with Priority Queue

**Intuition:**  
The idea is to make use of a priority queue (max-heap) to always execute the most frequent task first, ensuring the cooling period is properly respected.

**Steps:**  
1. Calculate the frequency of each task.
2. Push all task frequencies into a max-heap.
3. At each time unit, pop elements from the heap and attempt to execute the presumably optimal task.
4. If there are remaining tasks, keep track of their cooldown and continue.

**Code:**
```go
import (
    "container/heap"
)

func leastInterval(tasks []byte, n int) int {
    taskFreq := make(map[byte]int)
    for _, task := range tasks {
        taskFreq[task]++
    }

    // Initialize a max-heap
    h := &MaxHeap{}
    heap.Init(h)
    for _, freq := range taskFreq {
        heap.Push(h, freq)
    }

    intervals := 0
    for h.Len() > 0 {
        temp := []int{}
        for i := 0; i <= n; i++ {
            if h.Len() > 0 {
                // Pop the max frequency task
                freq := heap.Pop(h).(int)
                if freq-1 > 0 {
                    temp = append(temp, freq-1)
                }
            }
            intervals++
            if h.Len() == 0 && len(temp) == 0 {
                break
            }
        }
        for _, freq := range temp {
            heap.Push(h, freq)
        }
    }
    return intervals
}

// MaxHeap is a max-heap of int.
type MaxHeap []int

func (h MaxHeap) Len() int           { return len(h) }
func (h MaxHeap) Less(i, j int) bool { return h[i] > h[j] }
func (h MaxHeap) Swap(i, j int)      { h[i], h[j] = h[j], h[i] }

func (h *MaxHeap) Push(x interface{}) {
    *h = append(*h, x.(int))
}

func (h *MaxHeap) Pop() interface{} {
    old := *h
    n := len(old)
    x := old[n-1]
    *h = old[0 : n-1]
    return x
}
```
**Complexity Analysis:**
- Time Complexity: O(N log N), due to the heap operations.
- Space Complexity: O(N), for maintaining the heap.

## Approach 3: Greedy with Math Optimization

**Intuition:**  
The most optimal way is to rely on the mathematical formulation of the task scheduling problem. We can use the formula that accounts for the most frequent tasks and identify blocks they form. The use of idle times based on the cooling period will help fill intervals until the tasks are completed.

**Steps:**  
1. Calculate the frequency of each task to determine the maximum frequency.
2. Determine the number of tasks that have the maximum frequency.
3. Use the formula `result = max(len(tasks), (maxFreq - 1)*(n + 1) + maxCount)` to calculate the minimum intervals.

**Code:**
```go
func leastInterval(tasks []byte, n int) int {
    freq := make([]int, 26)
    for _, task := range tasks {
        freq[task-'A']++
    }

    maxFreq := 0
    maxCount := 0
    for _, f := range freq {
        if f > maxFreq {
            maxFreq = f
            maxCount = 1
        } else if f == maxFreq {
            maxCount++
        }
    }

    // Calculate the least interval using the formula
    partCount := maxFreq - 1
    partLength := n + 1
    emptySlots := partLength*partCount - (len(tasks) - maxFreq*maxCount)
    idleSlots := max(0, emptySlots)

    return len(tasks) + idleSlots
}

func max(x, y int) int {
    if x > y {
        return x
    }
    return y
}
```
**Complexity Analysis:**
- Time Complexity: O(N), where N is the number of tasks, even though calculation of max frequency is constant.
- Space Complexity: O(1), due to usage of a fixed-size frequency array.

The mathematical approach is the most optimal as it leverages an understanding of intervals for tasks to be distributed, providing direct calculation rather than iterative scheduling steps.

