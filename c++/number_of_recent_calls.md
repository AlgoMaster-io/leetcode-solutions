# [933. Number of Recent Calls](https://leetcode.com/problems/number-of-recent-calls/)

## Approach 1: Brute Force

### Solution
```cpp
// Time Complexity: O(n) per request (where n is the number of calls in the time window)
// Space Complexity: O(n)
#include <vector>

class RecentCounter {
private:
    std::vector<int> requests;

public:
    RecentCounter() {}

    int ping(int t) {
        requests.push_back(t); // Add the new request
        int count = 0;

        // Count all requests in the range [t - 3000, t]
        for (int time : requests) {
            if (time >= t - 3000) {
                count++;
            }
        }

        return count;
    }
};
```

## Approach 2: Using a Queue (Optimal Solution)

### Solution
```cpp
// Time Complexity: O(1) per request (amortized)
// Space Complexity: O(n)
#include <queue>

class RecentCounter {
private:
    std::queue<int> queue;

public:
    RecentCounter() {}

    int ping(int t) {
        queue.push(t); // Add the new request to the queue

        // Remove requests that are outside the range [t - 3000, t]
        while (!queue.empty() && queue.front() < t - 3000) {
            queue.pop();
        }

        return queue.size(); // The size of the queue represents the number of valid requests
    }
};
```

