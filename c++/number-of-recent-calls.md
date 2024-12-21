# [Leetcode 933: Number of Recent Calls](https://leetcode.com/problems/number-of-recent-calls/)

## Table of Contents
1. [Approach 1: Basic List-based Approach](#approach-1)
2. [Approach 2: Optimized Queue-based Approach](#approach-2)

## Approach 1: Basic List-based Approach <a name="approach-1"></a>

### Intuition
The problem asks for the number of recent calls one has made within the last 3000 milliseconds from the current time `t`. A naive way to solve this problem is by storing each call timestamp in a list and then calculating the count of timestamps that fall into the `[t-3000, t]` range for each query.

### Implementation

```cpp
class RecentCounter {
public:
    vector<int> timestamps;

    RecentCounter() {
        timestamps.clear();
    }
    
    int ping(int t) {
        timestamps.push_back(t);
        // Count how many timestamps are in the range [t - 3000, t]
        int count = 0;
        for(int time : timestamps) {
            if(time >= t - 3000 && time <= t) {
                count++;
            }
        }
        return count;
    }
};

/**
 * Your RecentCounter object will be instantiated and called as such:
 * RecentCounter* obj = new RecentCounter();
 * int param_1 = obj->ping(t);
```

### Complexity Analysis
- **Time Complexity**: `O(n)` for each query `ping`, where `n` is the total number of timestamps stored.
- **Space Complexity**: `O(n)`, to store the timestamps.

---

## Approach 2: Optimized Queue-based Approach <a name="approach-2"></a>

### Intuition
An efficient approach leverages the fact that for each `ping(t)`, we only care about timestamps within `[t - 3000, t]`. A queue (FIFO) data structure naturally supports efficient addition and removal of elements within this range. We'll maintain only the timestamps within the range `[t-3000, t]` in the queue by removing the ones that are outside, ensuring the queue contains only recent calls.

### Implementation

```cpp
#include <queue>
using namespace std;

class RecentCounter {
public:
    queue<int> q;

    RecentCounter() {
        while (!q.empty()) q.pop(); // Clear the queue
    }
    
    int ping(int t) {
        // Push the current timestamp
        q.push(t);

        // Remove all elements that are not within [t-3000, t]
        while (q.front() < t - 3000) {
            q.pop();
        }
        
        // The size of the queue gives the count of timestamps in the range [t-3000, t]
        return q.size();
    }
};

/**
 * Your RecentCounter object will be instantiated and called as such:
 * RecentCounter* obj = new RecentCounter();
 * int param_1 = obj->ping(t);
```

### Complexity Analysis
- **Time Complexity**: `O(1) average` time for each query `ping` assuming the average number of calls is evenly distributed over time, resulting in a constant amortized time to remove elements from the queue.
- **Space Complexity**: `O(w)`, where `w` is the maximum number of timestamps that can fall within a 3000 ms window. In a worst-case scenario, `w` can be at most proportional to the number of calls.

