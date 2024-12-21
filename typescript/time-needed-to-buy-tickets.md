# [Leetcode 2073: Time Needed to Buy Tickets](https://leetcode.com/problems/time-needed-to-buy-tickets/)

## Approaches
1. [Basic Simulation Approach](#basic-simulation-approach)
2. [Optimized Simulation Approach](#optimized-simulation-approach)

---

### Basic Simulation Approach

**Intuition**

We can simulate the process of buying tickets in a straightforward manner where we'll iterate over each person in the queue repeatedly. We decrement the number of tickets for each person by one for each "shift", counting the time until the targeted person (indexed `k`) has no more tickets to buy.

**Algorithm**  
1. Start a loop to simulate each second of the process sequentially.
2. For each person in the queue, if they still need tickets, decrement their count.
3. Track the time taken for each decrement.
4. Stop the process when the person `k` finishes buying all their tickets.
  
**Code**

```typescript
function timeNeededToBuy(tickets: number[], k: number): number {
    let time = 0;
    // Continue looping until the person at index k buys all their tickets
    while (tickets[k] > 0) {
        for (let i = 0; i < tickets.length; i++) {
            if (tickets[i] > 0) {
                tickets[i]--;  // Decrement ticket count for each person who still needs a ticket
                time++;  // Increment time count
            }
            // If the person at index k finished buying
            if (tickets[k] === 0) {
                return time;
            }
        }
    }
    
    return time;
}
```

**Complexity Analysis**

- **Time Complexity:** O(n * m), where `n` is the number of people in the queue, and `m` is the maximum number of tickets any person might want.
- **Space Complexity:** O(1), since we are just using additional variables for counting time and iterating through the list.

---

### Optimized Simulation Approach

**Intuition**

We can optimize the process by focusing on the key insight that each person gets at most `tickets[i]` turns. We need to track how many turns each person needs, but we don't need to simulate each second separately. We can smartly compute the total time more directly by looking at passes through the queue.

**Algorithm**
1. Calculate directly how many passes each person makes through the queue.
2. Sum up the time based on how many times each person actually buys a ticket.
3. Consider two cases: 
   - Persons before `k` contribute fully if they have more or equal tickets compared to `k`.
   - Persons after `k` also contribute until the person `k` is served fully.

**Code**

```typescript
function timeNeededToBuy(tickets: number[], k: number): number {
    let time = 0;
    
    for (let i = 0; i < tickets.length; i++) {
        if (i <= k) {
            // If the current index is less than or equal to k, sum the minimum of tickets[i] and tickets[k]
            time += Math.min(tickets[i], tickets[k]);
        } else {
            // If the current index is greater than k, sum the minimum of tickets[i] and tickets[k] - 1
            time += Math.min(tickets[i], tickets[k] - 1);
        }
    }

    return time;
}
```

**Complexity Analysis**

- **Time Complexity:** O(n), where `n` is the number of people in the queue as we traverse the queue only once.
- **Space Complexity:** O(1), because we are using only a constant amount of extra space for variables.

This approach efficiently calculates the required time by leveraging the understanding of how the queue behaves relative to the person at index `k`, significantly reducing the simulation overhead.

