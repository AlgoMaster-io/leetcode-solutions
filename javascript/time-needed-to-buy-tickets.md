## [LeetCode 2073: Time Needed to Buy Tickets](https://leetcode.com/problems/time-needed-to-buy-tickets/)

### Approaches
1. [Brute Force Approach](#approach-1-brute-force-approach)
2. [Optimized Approach with Decrement Simulation](#approach-2-optimized-approach-with-decrement-simulation)

### Approach 1: Brute Force Approach
#### Intuition:
This problem can be approached by simulating each ticket purchase on an individual basis. We simply simulate each person buying each ticket until the person at the given index finishes buying their tickets.

#### Steps:
- We'll create a simple loop structure that decrements the number of tickets each person needs, one at a time.
- For each single unit of time taken, decrement the ticket count for the person currently at the front of the queue (circulating through the array to simulate a queue behavior).
- Keep track of time by incrementing it each time someone successfully "buys" a ticket.
- Stop when the person at the given `k` index has finished buying all their tickets.

```javascript
function timeRequiredToBuy(tickets, k) {
    let time = 0;
    let n = tickets.length;
    
    while (tickets[k] > 0) {
        for (let i = 0; i < n; i++) {
            // Only decrement if the person still has tickets needed
            if (tickets[i] > 0) {
                tickets[i]--;
                time++;
            }
            // Stop when the person at index k is finished
            if (tickets[k] === 0) {
                return time;
            }
        }
    }
    
    return time;
}
```

#### Complexity Analysis:
- **Time Complexity**: O(n * m), where `n` is the number of people and `m` is the maximum number of tickets anyone can have.
- **Space Complexity**: O(1), no extra space other than variables.

### Approach 2: Optimized Approach with Decrement Simulation
#### Intuition:
We can optimize the simulation by avoiding looping through all tickets unnecessarily. Instead, we focus on reducing the number of times we need to simulate unnecessary purchases.

#### Steps:
- Calculate separately how many tickets each person in queue will contribute to `time` by the time person `k` finishes their tickets.
- Before `k`, each person gets diminished by `min(tickets[i], tickets[k])`, meaning if a person has fewer tickets than `k`, they will add their full ticket count to time, otherwise they only contribute up till `tickets[k]`.
- After `k`, each subsequent person at most contributes `tickets[k] - 1` since the loop would break right after `k` gets to zero.

```javascript
function timeRequiredToBuy(tickets, k) {
    let time = 0;
    let n = tickets.length;
    
    for (let i = 0; i < n; i++) {
        if (i <= k) {
            time += Math.min(tickets[i], tickets[k]);
        } else {
            time += Math.min(tickets[i], tickets[k] - 1);
        }
    }
    
    return time;
}
```

#### Complexity Analysis:
- **Time Complexity**: O(n), where `n` is the number of people. We only loop through the list once.
- **Space Complexity**: O(1), as we only use a constant amount of space for variables.

This optimized version drastically reduces redundant simulations, making it far more efficient for larger input sizes.

