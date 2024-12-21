# [LeetCode 502: IPO](https://leetcode.com/problems/ipo/)

## Approaches
- [Approach 1: Brute Force Selection](#approach-1-brute-force-selection)
- [Approach 2: Sorting and Priority Queue](#approach-2-sorting-and-priority-queue)

### Approach 1: Brute Force Selection

**Intuition:**

The brute force approach involves simulating the selection process for projects. We consider each project multiple times, ensuring we don't select projects with a capital requirement greater than our current capital.

**Steps:**

1. Track our current available capital.
2. For up to `k` times (the number of projects we can select), look through all projects.
3. Select the project that can be completed with current capital and provides the maximum profit.
4. Add the profit of the selected project to current capital and mark the project as completed.
5. Repeat until either we can't find any project that can be completed with the current capital or we've selected `k` projects.

**Code:**

```javascript
/**
 * @param {number} k
 * @param {number} W
 * @param {number[]} Profits
 * @param {number[]} Capital
 * @return {number}
 */
var findMaximizedCapital = function(k, W, Profits, Capital) {
    let n = Profits.length;
    let projects = [];
    
    // Merge projects and capital into a single list of objects
    for (let i = 0; i < n; i++) {
        projects.push({ profit: Profits[i], capital: Capital[i] });
    }
    
    while (k > 0) {
        // Find the project with maximum profit within our capital
        let maxProfit = -1;
        let index = -1;

        for (let i = 0; i < projects.length; i++) {
            if (projects[i] && projects[i].capital <= W) {
                if (projects[i].profit > maxProfit) {
                    maxProfit = projects[i].profit;
                    index = i;
                }
            }
        }

        // If no project is available, break
        if (index === -1) {
            break;
        }

        // Increase the capital by the profit of the chosen project
        W += projects[index].profit;
        // Remove the project from consideration
        projects[index] = null;
        k--;
    }
    
    return W;
};
```

**Time Complexity:** O(n * k), where n is the number of projects and k is the number of selections allowed.  
**Space Complexity:** O(n), to store the projects in a list.

### Approach 2: Sorting and Priority Queue

**Intuition:**

To improve efficiency, we use a combination of a sorted list and a priority queue. We first sort the projects based on their capital requirements. We use a max-heap (or priority queue) to keep track of the projects we can currently afford and select the one with the maximum profit.

**Steps:**

1. Sort all projects based on their capital requirements.
2. Iterate through the list of projects:
   - Using the current capital, add all projects satisfying the capital requirement into a max-heap based on profit.
3. Extract the maximum profit project from the heap, add its profit to the current capital, and continue to the next selection.
4. Repeat this process up to `k` times or until no more projects can be added.

**Code:**

```javascript
/**
 * @param {number} k
 * @param {number} W
 * @param {number[]} Profits
 * @param {number[]} Capital
 * @return {number}
 */
var findMaximizedCapital = function(k, W, Profits, Capital) {
    let projects = [];
    
    // Merge profits and capital into an array of project objects
    for (let i = 0; i < Profits.length; i++) {
        projects.push({ profit: Profits[i], capital: Capital[i] });
    }
    
    // Sort projects by increasing order of capital
    projects.sort((a, b) => a.capital - b.capital);
    
    // Max-heap for storing the profits of affordable projects
    let maxProfitHeap = new MaxPriorityQueue({ priority: (project) => project.profit });
    let index = 0;

    // Loop up to k times to select projects
    for (let i = 0; i < k; i++) {
        // Add all projects that can be afforded with current capital to the heap
        while (index < projects.length && projects[index].capital <= W) {
            maxProfitHeap.enqueue(projects[index]);
            index++;
        }

        // If no projects are affordable, break
        if (maxProfitHeap.isEmpty()) {
            break;
        }

        // Extract the project with max profit
        W += maxProfitHeap.dequeue().element.profit;
    }
    
    return W;
};
```

**Time Complexity:** O(n log n + k log n), due to sorting the projects and maintaining the heap.  
**Space Complexity:** O(n), for the project list and heap.

