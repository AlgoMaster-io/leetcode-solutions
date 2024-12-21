# [LeetCode 502: IPO](https://leetcode.com/problems/ipo/)

## Approaches
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Priority Queue (Max Heap)](#approach-2-priority-queue-max-heap)

---

## Approach 1: Brute Force

### Intuition
In the brute force approach, we consider investing our initial capital into projects that we can afford and then add the profit to our current capital. For each project considered, we have to check all projects we can afford and pick the one with the maximum profit. This exhaustive process leads to a higher time complexity.

### Approach
1. Start with the available initial capital.
2. For each round, evaluate all available projects that we can afford with the current capital.
3. Choose the project with the maximum profit and increment the capital.
4. Repeat the process for at most `k` investments.

### Code
```cpp
class Solution {
public:
    int findMaximizedCapital(int k, int W, vector<int>& Profits, vector<int>& Capital) {
        int n = Profits.size();
        vector<bool> used(n, false);
        
        for (int i = 0; i < k; ++i) { // for at most k investments
            int maxProf = -1, index = -1;
            for (int j = 0; j < n; ++j) { // check every project
                if (!used[j] && Capital[j] <= W && Profits[j] > maxProf) {
                    maxProf = Profits[j];
                    index = j;
                }
            }
            if (index == -1) break; // no project can be taken
            W += Profits[index];
            used[index] = true;
        }
        
        return W;
    }
};
```

### Time Complexity
- **Time Complexity**: O(kn), where `k` is the maximum number of projects we can choose and `n` is the number of projects.
- **Space Complexity**: O(n), used to keep track of which projects have been used.

---

## Approach 2: Priority Queue (Max Heap)

### Intuition
To optimize the selection of projects, we utilize a max heap data structure to always pick the project with the maximum profit that we can afford next. This way, finding the next most profitable project becomes more efficient as compared to the brute force approach.

### Approach
1. Pair the profits with capital requirements and sort based on capital requirements.
2. Use a max heap (priority queue) to store available projects.
3. Iterate through the projects and push all affordable projects into the heap.
4. Extract the project with the maximum profit from the heap until `k` or no more projects can be afforded.

### Code
```cpp
#include <queue>
#include <vector>
#include <algorithm>

class Solution {
public:
    int findMaximizedCapital(int k, int W, vector<int>& Profits, vector<int>& Capital) {
        int n = Profits.size();
        vector<pair<int, int>> projects;
        for (int i = 0; i < n; ++i) {
            projects.push_back({Capital[i], Profits[i]});
        }
        // Sort projects by their capital requirements
        sort(projects.begin(), projects.end());
        
        priority_queue<int> maxHeap;
        int i = 0;
        
        for (int j = 0; j < k; ++j) { // for at most k investments
            // Push all projects that can be afforded into the max heap
            while (i < n && projects[i].first <= W) {
                maxHeap.push(projects[i].second);
                i++;
            }
            if (maxHeap.empty()) break; // No projects can be taken anymore
            
            // Take the project with the maximum profit
            W += maxHeap.top();
            maxHeap.pop();
        }
        
        return W;
    }
};
```

### Time Complexity
- **Time Complexity**: O(n log n + k log n), where `n` is the number of projects. Sorting the projects takes `O(n log n)`, and extracting from the heap is `O(log n)`.
- **Space Complexity**: O(n), for storing pairs and using the max heap.

By employing these approaches strategically, we efficiently decide on projects that maximize our capital growth.

