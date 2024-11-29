# [2073. Time Needed to Buy Tickets](https://leetcode.com/problems/time-needed-to-buy-tickets/)

## Approach 1: Simulation with a Loop

### Solution
typescript
```typescript
// Time Complexity: O(n * max(tickets))
// Space Complexity: O(1)
class Solution {
    timeRequiredToBuy(tickets: number[], k: number): number {
        let time = 0;

        // Simulate the ticket-buying process
        while (tickets[k] > 0) {
            for (let i = 0; i < tickets.length; i++) {
                if (tickets[i] > 0) {
                    tickets[i]--; // Person buys one ticket
                    time++; // Increment time for each ticket bought
                }
                // Stop as soon as the person at index k finishes buying
                if (tickets[k] === 0) {
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
typescript
```typescript
// Time Complexity: O(n)
// Space Complexity: O(1)
class Solution {
    timeRequiredToBuy(tickets: number[], k: number): number {
        let time = 0;

        // Iterate through all the people in line
        for (let i = 0; i < tickets.length; i++) {
            // Add the minimum tickets this person can buy
            time += i <= k ? Math.min(tickets[i], tickets[k]) : Math.min(tickets[i], tickets[k] - 1);
        }

        return time;
    }
}
```

