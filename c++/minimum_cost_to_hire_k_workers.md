# 857. [Minimum Cost to Hire K Workers](https://leetcode.com/problems/minimum-cost-to-hire-k-workers/)

## Approach: Sorting and Priority Queue (Max-Heap)

### Solution
```cpp
// Time Complexity: O(n * log(n) + n * log(k)), where n is the number of workers and k is the number of workers to hire
// Space Complexity: O(k), for the max-heap
#include <vector>
#include <algorithm>
#include <queue>
#include <limits>

using namespace std;

class Solution {
public:
    double mincostToHireWorkers(vector<int>& quality, vector<int>& wage, int k) {
        int n = quality.size();
        vector<pair<double, int>> workers(n);

        // Create a vector of workers with their wage-to-quality ratio and quality
        for (int i = 0; i < n; i++) {
            workers[i] = {(double)wage[i] / quality[i], quality[i]}; // Wage-to-quality ratio
        }

        // Sort workers by their wage-to-quality ratio
        sort(workers.begin(), workers.end());

        priority_queue<int> maxHeap; // Max-heap for quality
        int totalQuality = 0; // Sum of qualities in the current group
        double minCost = numeric_limits<double>::infinity();

        for (const auto& worker : workers) {
            int currentQuality = worker.second;
            totalQuality += currentQuality;
            maxHeap.push(currentQuality);

            // If we exceed k workers, remove the one with the highest quality
            if (maxHeap.size() > k) {
                totalQuality -= maxHeap.top();
                maxHeap.pop();
            }

            // If we have exactly k workers, calculate the cost
            if (maxHeap.size() == k) {
                minCost = min(minCost, totalQuality * worker.first);
            }
        }

        return minCost;
    }
};
```

