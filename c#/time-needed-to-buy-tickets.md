# [2073. Time Needed to Buy Tickets](https://leetcode.com/problems/time-needed-to-buy-tickets/)

## Approaches

1. [Brute Force Simulation](#brute-force-simulation)
2. [Optimized Queue Simulation](#optimized-queue-simulation)

---

### Brute Force Simulation

**Intuition**:  
The goal is to simulate the process of buying tickets as described. Each person can buy one ticket per unit of time, and they stand in a queue which moves cyclically. We start with a simple brute force approach where we simulate each unit of time explicitly until the target person has their tickets fully processed.

#### Steps:
1. Initialize a variable to track the total time taken (`time = 0`).
2. Use a `while` loop to iterate until the target person's tickets (`tickets[k]`) are exhausted.
3. In each iteration, loop through the `tickets` array:
   - If a person (`tickets[i] > 0`) is in line, decrement their ticket count and increment the time.
   - Break the loop if the target person's tickets are exhausted within the loop.
4. The time when the loop exits is the total time taken for the target person to buy their tickets.

```csharp
public class Solution {
    public int TimeRequiredToBuy(int[] tickets, int k) {
        int time = 0;
        while (tickets[k] > 0) { // Continue until the target person has no tickets left
            for (int i = 0; i < tickets.Length; i++) {
                if (tickets[i] > 0) { // If a person still has tickets to buy
                    tickets[i]--; // Simulate buying one ticket
                    time++; // Increment the time
                }
                if (tickets[k] == 0) { // Stop if target person has finished buying
                    break;
                }
            }
        }
        return time;
    }
}
```

**Time Complexity**: O(n * max(tickets)), where n is the number of people, and max(tickets) is the maximum number of tickets any single person has.  
**Space Complexity**: O(1), as no additional space proportional to input size is needed beyond simple counters.

---

### Optimized Queue Simulation

**Intuition**:
Instead of iterating through all persons repeatedly, we can reduce unnecessary checks by limiting operations to only as much as needed for the target person. We recognize that:
- People ahead of `k` in the queue can contribute to the time up to the minimum of their tickets and `tickets[k]`.
- People behind `k` can at most add time equivalent to their ticket count if it's less than `tickets[k]`.

#### Steps:
1. Initialize `time` counter.
2. Traverse the queue.
3. If the current person index is less than or equal to `k`, add the minimum of `tickets[k]` and their ticket count to the `time`.
4. If the current person index is greater than `k`, add the minimum of `tickets[k] - 1` and their tickets to the `time` because they can get only up to one less than the target's count.
5. Return the `time`.

```csharp
public class Solution {
    public int TimeRequiredToBuy(int[] tickets, int k) {
        int time = 0;
        for (int i = 0; i < tickets.Length; i++) {
            if (i <= k) {
                time += Math.Min(tickets[i], tickets[k]); // Upper bound on those ahead or at k
            } else {
                time += Math.Min(tickets[i], tickets[k] - 1); // Adjust for those after k
            }
        }
        return time;
    }
}
```

**Time Complexity**: O(n), where n is the number of people since we iterate through the list only once.  
**Space Complexity**: O(1), as no additional storage proportional to the input is required.

