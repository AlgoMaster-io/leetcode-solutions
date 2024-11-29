# 134. [Gas Station](https://leetcode.com/problems/gas-station/)

## Approach: Greedy Algorithm

### Solution
```javascript
// Time Complexity: O(n), where n is the number of gas stations
// Space Complexity: O(1)
var canCompleteCircuit = function(gas, cost) {
    let totalGas = 0; // Total gas across all stations
    let totalCost = 0; // Total cost across all stations
    let startStation = 0; // Potential starting station
    let currentGas = 0; // Gas balance while traversing

    for (let i = 0; i < gas.length; i++) {
        totalGas += gas[i];
        totalCost += cost[i];
        currentGas += gas[i] - cost[i];

        // If currentGas is negative, reset the starting station
        if (currentGas < 0) {
            startStation = i + 1;
            currentGas = 0; // Reset gas balance
        }
    }

    // If total gas is less than total cost, completing the circuit is impossible
    return totalGas >= totalCost ? startStation : -1;
};
```

