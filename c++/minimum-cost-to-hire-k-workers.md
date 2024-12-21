# [Minimum Cost to Hire K Workers - LeetCode](https://leetcode.com/problems/minimum-cost-to-hire-k-workers/)

## Approaches:
1. [Greedy with Sorting](#greedy-with-sorting)

---

### Greedy with Sorting

**Intuition:**

The problem boils down to minimizing the total cost for hiring exactly K workers. Each worker has a minimum ratio (wage/quality) they are willing to accept. The key insight is that for any group of workers, the cost is determined by the worker with the highest wage-to-quality ratio in that group, as everyone else will be fine with this wage as it is at least what they demand. Thus, the strategy involves sorting workers by their wage-to-quality ratio, then iterating through workers to find the minimum cost using a heap (priority queue) to maintain the K workers with the highest quality, ensuring that any potential group's cost can be minimized.

**Steps:**
1. Compute the wage-to-quality ratio for each worker.
2. Sort workers based on this ratio.
3. Use a heap to maintain the top K smallest qualities from the first N workers as we make our selections. This ensures we minimize the total cost for any group containing K workers.
4. As you iterate through the sorted workers:
   - Add current worker's quality to the heap.
   - If the heap exceeds size K, remove the worker with the highest quality (since higher quality drives up the cost).
5. For each iteration, compute the potential cost using the current worker’s ratio and the sum of K smallest qualities.
6. Keep track of the minimum cost encountered.

**Code:**

```cpp
#include <vector>
#include <queue>
#include <algorithm>
#include <functional>

using namespace std;

class Solution {
public:
    double mincostToHireWorkers(vector<int>& quality, vector<int>& wage, int K) {
        vector<pair<double, int>> workers;
        int num_workers = quality.size();
        
        // Create a vector of pairs (wage-to-quality ratio, quality)
        for (int i = 0; i < num_workers; ++i) {
            double ratio = (double)wage[i] / quality[i];
            workers.push_back({ratio, quality[i]});
        }
        
        // Sort based on the ratio
        sort(workers.begin(), workers.end());
        
        // Minimize the total cost using a max heap
        priority_queue<int> max_heap;
        int total_quality = 0;
        double min_cost = numeric_limits<double>::max();
        
        for (const auto& worker : workers) {
            double ratio = worker.first;
            int qual = worker.second;
            
            // Add the quality to the total quality and the max heap
            total_quality += qual;
            max_heap.push(qual);
            
            // If we have more than K workers, remove the one with the highest quality
            if (max_heap.size() > K) {
                total_quality -= max_heap.top();
                max_heap.pop();
            }
            
            // If we have exactly K workers, calculate the cost
            if (max_heap.size() == K) {
                double cost = ratio * total_quality;
                min_cost = min(min_cost, cost);
            }
        }
        
        return min_cost;
    }
};
```

**Complexity Analysis:**
- **Time Complexity:** \(O(N \log N)\), where \(N\) is the number of workers since we need to sort the workers based on their ratio.
- **Space Complexity:** \(O(N)\) due to the use of a priority queue to store qualities. 

This approach efficiently selects the group of K workers such that the cost is minimized using a greedy strategy informed by sorting and heap operations.

