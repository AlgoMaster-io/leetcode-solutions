# [2073. Time Needed to Buy Tickets](https://leetcode.com/problems/time-needed-to-buy-tickets/)

## Approach 1: Simulation with a Loop

### Solution
```java
// Time Complexity: O(n * max(tickets))
// Space Complexity: O(1)
public class Solution {
    public int timeRequiredToBuy(int[] tickets, int k) {
        int time = 0;

        // Simulate the ticket-buying process
        while (tickets[k] > 0) {
            for (int i = 0; i < tickets.length; i++) {
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
```java
// Time Complexity: O(n)
// Space Complexity: O(1)
public class Solution {
    public int timeRequiredToBuy(int[] tickets, int k) {
        int time = 0;

        // Iterate through all the people in line
        for (int i = 0; i < tickets.length; i++) {
            // Add the minimum tickets this person can buy
            if (i <= k) {
                time += Math.min(tickets[i], tickets[k]);
            } else {
                time += Math.min(tickets[i], tickets[k] - 1);
            }
        }

        return time;
    }
}
```