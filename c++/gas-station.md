### [Leetcode Problem 134: Gas Station](https://leetcode.com/problems/gas-station/)

#### Approaches
- [Brute Force Approach](#brute-force-approach)
- [Optimized Greedy Approach](#optimized-greedy-approach)

---

### Brute Force Approach

**Intuition:**

The brute force approach involves trying to start at each gas station and checking if we can complete the circuit. For each starting station, we simulate the journey in a circular manner, keeping track of the gas remaining at each step. The process continues until either we complete the journey or run out of gas.

**Algorithm Steps:**

1. For each gas station as the starting point:
    - Initialize current gas to zero.
    - Simulate traveling from this station through each subsequent station.
    - If at any station, gas becomes negative, break and try the next starting station.
    - If the journey can be completed, return the starting station index.

**Code:**

```cpp
#include <vector>

class Solution {
public:
    int canCompleteCircuit(std::vector<int>& gas, std::vector<int>& cost) {
        int n = gas.size();
        
        // Try starting from each station
        for (int start = 0; start < n; ++start) {
            int currentGas = 0;
            bool canComplete = true;

            // Simulate journey
            for (int i = 0; i < n; ++i) {
                int index = (start + i) % n;  // Circular index
                currentGas += gas[index] - cost[index];
                
                // If at any point the gas is negative, break
                if (currentGas < 0) {
                    canComplete = false;
                    break;
                }
            }
            
            // If journey is successful, return start index
            if (canComplete) return start;
        }
        
        return -1;  // If no start point is feasible
    }
};
```

- **Time Complexity**: \(O(n^2)\) - Since for each station, we might traverse all stations.
- **Space Complexity**: \(O(1)\) - No additional space apart from variables.

---

### Optimized Greedy Approach

**Intuition:**

The optimized solution exploits the fact that if starting from a station \(i\) leaves us with negative gas at some station \(j\), then none of the stations between \(i\) and \(j\) can be starting points. Therefore, we skip all these stations and set \(j + 1\) as the next starting station. Moreover, for a solution to exist, the total amount of gas must be at least the total cost.

**Algorithm Steps:**

1. Calculate total gas and total cost.
2. If total gas is less than total cost, return -1 (impossible to complete circuit).
3. Initialize variables to track starting station, current gas, total gas, and total cost.
4. Iterate over the stations:
    - Update current gas at each step.
    - If current gas falls below zero, update starting station to the next station and reset current gas.
5. If possible to complete the circuit, the update starting station will be the answer.

**Code:**

```cpp
#include <vector>

class Solution {
public:
    int canCompleteCircuit(std::vector<int>& gas, std::vector<int>& cost) {
        int totalGas = 0, currentGas = 0, startStation = 0;
        
        for (int i = 0; i < gas.size(); ++i) {
            totalGas += gas[i] - cost[i];
            currentGas += gas[i] - cost[i];
            
            // If the current gas sum is negative, reset start station past i
            if (currentGas < 0) {
                startStation = i + 1;
                currentGas = 0;
            }
        }
        
        // Check if the total gas sum is non-negative, which allows completing the circuit
        return totalGas >= 0 ? startStation : -1;
    }
};
```

- **Time Complexity**: \(O(n)\) - Single pass over the stations.
- **Space Complexity**: \(O(1)\) - Uses a constant amount of extra space.

This solution efficiently determines the starting station by leveraging the insight into excess and deficit of gas as we traverse the stations.

