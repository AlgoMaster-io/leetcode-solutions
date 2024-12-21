# [Gas Station](https://leetcode.com/problems/gas-station/)

## Approaches
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Greedy Approach](#approach-2-greedy-approach)

## Approach 1: Brute Force

### Intuition
A straightforward way to solve this problem is to try starting from each gas station and check if we can complete the circuit. This involves iterating for every station and attempting to travel around the circuit, keeping track of the gas left. If we can make it around the circuit, return the starting index. If no such starting index exists, return -1.

### Steps
1. Iterate through each gas station and consider it as a potential starting point.
2. For each starting point, see if the car can complete the loop.
3. Calculate the remaining gas at each step, and if it ever becomes negative during a trial, break out and try the next starting point.
4. If you complete the circuit from any starting point, return that index.
5. If no starting point completes the circuit, return -1.

### Code
```go
func canCompleteCircuit(gas []int, cost []int) int {
    n := len(gas)
    for start := 0; start < n; start++ {
        fuel := 0
        for i := 0; i < n; i++ {
            idx := (start + i) % n
            fuel += gas[idx] - cost[idx]
            if fuel < 0 {
                break
            }
        }
        if fuel >= 0 {
            return start
        }
    }
    return -1
}
```

### Complexity Analysis
- **Time Complexity:** O(n^2). For each station, we check if the circuit can be completed, which could involve a complete loop over n stations.
- **Space Complexity:** O(1). No additional space is required apart from variables.

---

## Approach 2: Greedy Approach

### Intuition
A more optimal solution utilizes a greedy approach. The main trick is if a car starts at A and fails at B, then any station between A and B should not be considered as a starting point. This is because the total gas between A and B is less than the total cost, thereby making it impossible to make it from any station within that range. Hence, we only need to track the cumulative sum and reset the starting station accordingly.

### Steps
1. Initialize total_gas and total_cost to keep track of the net gas and cost.
2. Initialize current_gas to track the gas relative to current possible start_position.
3. Initialize start_position to 0.
4. Loop over each station:
   - Update total_gas and total_cost.
   - Update current_gas with gas_bought and cost at current station. 
   - If current_gas is negative, reset start_position to the next station and reset current_gas.
5. If after the entire circuit, total_gas is less than total_cost, return -1; otherwise, return start_position.

### Code
```go
func canCompleteCircuit(gas []int, cost []int) int {
    totalGas, totalCost, currentGas, start := 0, 0, 0, 0
    for i := 0; i < len(gas); i++ {
        totalGas += gas[i]
        totalCost += cost[i]
        currentGas += gas[i] - cost[i]
        // If the current gas is negative, it means we can't start from current start
        if currentGas < 0 {
            start = i + 1
            currentGas = 0
        }
    }
    if totalGas < totalCost {
        return -1
    }
    return start
}
```

### Complexity Analysis
- **Time Complexity:** O(n). We traverse the gas and cost arrays once.
- **Space Complexity:** O(1). We only use a few variables for tracking.

