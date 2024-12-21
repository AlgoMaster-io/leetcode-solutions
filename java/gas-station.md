# [Leetcode 134: Gas Station](https://leetcode.com/problems/gas-station/)

## Solutions:
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Improved Efficient Solution with O(n) complexity](#approach-2-improved-efficient-solution)

---

### Approach 1: Brute Force

#### Intuition
The Gas Station problem requires us to find a starting gas station, if it exists, from which we can complete a circular route. The brute force solution involves trying every gas station as a starting point and checking if a circular journey can be completed. This involves calculating the remaining gas from each station, updating the current gas as you proceed, and checking if you can reach back to the starting station.

#### Java Solution
```java
public class Solution {
    public int canCompleteCircuit(int[] gas, int[] cost) {
        int n = gas.length;
        
        // Try to start from each gas station
        for (int start = 0; start < n; start++) {
            int tank = 0;
            int count = 0;
            int i = start;
            boolean canComplete = true;
            
            // Traverse around the circuit starting from 'start'
            while (count < n) {
                tank += gas[i] - cost[i];
                
                // If at any point the tank is negative, break and try next starting point
                if (tank < 0) {
                    canComplete = false;
                    break;
                }
                
                // Move to the next station, circling back if necessary
                i = (i + 1) % n;
                count++;
            }
            
            // If we managed to complete the circuit, return the starting point
            if (canComplete) {
                return start;
            }
        }
        
        // If no valid starting point is found, return -1
        return -1;
    }
}
```

#### Time Complexity
- O(n^2) because for each starting gas station, we potentially traverse the entire circuit.

#### Space Complexity
- O(1), no additional space is used apart from variables.

---

### Approach 2: Improved Efficient Solution

#### Intuition
The improved solution takes advantage of certain observations:
1. If the total gas is greater than the total cost, then a solution must exist.
2. If at any station, the current gas becomes negative, it indicates this station cannot be a starting point, and all stations before it are also not viable.

We traverse through each station once, keeping track of the total surplus and current surplus. Whenever the current surplus becomes negative, we reset the potential start point to the next station.

#### Java Solution
```java
public class Solution {
    public int canCompleteCircuit(int[] gas, int[] cost) {
        int totalSurplus = 0;
        int currentSurplus = 0;
        int start = 0;

        for (int i = 0; i < gas.length; i++) {
            totalSurplus += gas[i] - cost[i];
            currentSurplus += gas[i] - cost[i];
            
            // If current surplus drops below 0, the start is not valid, move to next station
            if (currentSurplus < 0) {
                start = i + 1;
                currentSurplus = 0;
            }
        }
        
        // If the total surplus is non-negative, the circuit can be completed
        return totalSurplus >= 0 ? start : -1;
    }
}
```

#### Time Complexity
- O(n) because we make a single pass through the gas stations and costs.

#### Space Complexity
- O(1), no additional space is used other than basic variables.

In conclusion, this efficient approach lets us determine the start station in linear time by exploiting the properties of gas and cost relationships.

