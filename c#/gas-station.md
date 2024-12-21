[Leetcode Problem 134: Gas Station](https://leetcode.com/problems/gas-station/)

## Approaches:

1. [Brute Force Solution](#brute-force-solution)
2. [Optimized Greedy Solution](#optimized-greedy-solution)

---

### Brute Force Solution

#### Intuition
The brute force approach considers each gas station as a potential starting point and simulates the process to see if the car can travel around the circuit back to the starting point without running out of gas. This is achieved by checking if, starting from each station, the car can make a full circle.

#### Steps:
- For each gas station, simulate driving around the circuit:
  - Initialize your `remainingGas` with the gas from the starting station.
  - As you reach each station, compute and adjust the remaining gas after subtracting the cost to reach the next station.
  - If you ever have negative `remainingGas`, break out of the loop and try the next starting station.
  
#### Code

```csharp
public class Solution {
    public int CanCompleteCircuit(int[] gas, int[] cost) {
        int n = gas.Length;
        for (int start = 0; start < n; start++) {
            int remainingGas = 0;
            int count = 0;
            while (count < n) {
                int currentStation = (start + count) % n;
                remainingGas += gas[currentStation] - cost[currentStation];
                if (remainingGas < 0) {
                    break;
                }
                count++;
            }
            if (count == n) {
                return start;
            }
        }
        return -1;
    }
}
```

#### Complexity
- **Time Complexity**: O(n^2), due to having to re-simulate from each station.
- **Space Complexity**: O(1), since we do not use any additional space proportional to input size.

---

### Optimized Greedy Solution

#### Intuition
The optimal solution observes that if there is a solution, it must be unique. Thus, if at any point the car cannot make it from station `i` to `i+1`, then no station between 0 and i can be a valid start. This realization allows us to efficiently find the starting point.

#### Steps:
- Track the total net gas available and use it to verify if the solution exists.
- Track `remainingGas` as you iterate through the stations.
- If `remainingGas` becomes negative while iterating, set the next station as the new starting point candidate since it indicates the current segment cannot be a valid start.
- If the total net gas for the entire circuit is negative, return -1 meaning the trip is not possible.

#### Code

```csharp
public class Solution {
    public int CanCompleteCircuit(int[] gas, int[] cost) {
        int totalGas = 0;
        int remainingGas = 0;
        int start = 0;

        for (int i = 0; i < gas.Length; i++) {
            int netGas = gas[i] - cost[i];
            totalGas += netGas;
            remainingGas += netGas;

            // If remaining gas is less than zero, reset starting point
            if (remainingGas < 0) {
                start = i + 1;
                remainingGas = 0;
            }
        }

        return totalGas >= 0 ? start : -1;
    }
}
```

#### Complexity
- **Time Complexity**: O(n), only a single pass through the stations is required.
- **Space Complexity**: O(1), only a few extra variables are used.

---

This concludes the solutions to the Gas Station problem, exposing both the brute force way and the optimal greedy approach.

