# [2073. Time Needed to Buy Tickets](https://leetcode.com/problems/time-needed-to-buy-tickets/)

## Approach 1: Simulation with a Loop

### Solution
csharp
```csharp
// Time Complexity: O(n * max(tickets))
// Space Complexity: O(1)
public class Solution {
    public int TimeRequiredToBuy(int[] tickets, int k) {
        int time = 0;

        // Simulate the ticket-buying process
        while (tickets[k] > 0) {
            for (int i = 0; i < tickets.Length; i++) {
                if (tickets[i] > 0) {
                    tickets[i]--; // Person buys one ticket
                    time++; // Increment time for each ticket bought
                }
                // Stop as soon as the person at index k finishes buying
                if (tickets[k] == 0) {
                    return time;
                }
            }
        }

        return time;
    }
}
```

## Approach 2: Optimized Calculation

### Solution
csharp
```csharp
// Time Complexity: O(n)
// Space Complexity: O(1)
public class Solution {
    public int TimeRequiredToBuy(int[] tickets, int k) {
        int time = 0;

        // Iterate through all the people in line
        for (int i = 0; i < tickets.Length; i++) {
            // Add the minimum tickets this person can buy
            if (i <= k) {
                time += Math.Min(tickets[i], tickets[k]);
            } else {
                time += Math.Min(tickets[i], tickets[k] - 1);
            }
        }

        return time;
    }
}
```

