# [2073. Time Needed to Buy Tickets](https://leetcode.com/problems/time-needed-to-buy-tickets/)

## Approach 1: Simulation with a Loop

### Solution
```python
# Time Complexity: O(n * max(tickets))
# Space Complexity: O(1)
class Solution:
    def timeRequiredToBuy(self, tickets, k):
        time = 0

        # Simulate the ticket-buying process
        while tickets[k] > 0:
            for i in range(len(tickets)):
                if tickets[i] > 0:
                    tickets[i] -= 1 # Person buys one ticket
                    time += 1 # Increment time for each ticket bought
                # Stop as soon as the person at index k finishes buying
                if tickets[k] == 0:
                    return time

        return time
```

## Approach 2: Optimized Calculation

### Solution
```python
# Time Complexity: O(n)
# Space Complexity: O(1)
class Solution:
    def timeRequiredToBuy(self, tickets, k):
        time = 0

        # Iterate through all the people in line
        for i in range(len(tickets)):
            # Add the minimum tickets this person can buy
            if i <= k:
                time += min(tickets[i], tickets[k])
            else:
                time += min(tickets[i], tickets[k] - 1)

        return time
```

