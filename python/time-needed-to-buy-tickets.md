# [Leetcode 2073: Time Needed to Buy Tickets](https://leetcode.com/problems/time-needed-to-buy-tickets/)

## Approaches:
- [Approach 1: Simulation with Queue](#approach-1-simulation-with-queue)

---

## Approach 1: Simulation with Queue

### Intuition
The problem can be understood as simulating a ticket buying process where each person in line buys one ticket at a time until they have all they need. We will simulate this process by iterating over the queue of people, decrementing their ticket needs until the person at the index `k` is satisfied. The critical part is to keep track of how many steps it takes for the `k-th` person to finish buying their tickets.

### Detailed Steps
1. Initialize a queue representing the people's ticket needs.
2. Use a loop to simulate the ticket buying process:
   - Iterate over every person in the queue, allowing them to buy one ticket at a time.
   - Decrement the ticket count for each person if they still need more.
   - Keep a counter for the number of steps taken.
   - Check after each decrement if the `k-th` person has been fully served.
3. When the `k-th` person doesn't need more tickets, output the number of steps taken.

### Code

```python
def timeRequiredToBuy(tickets, k):
    time = 0  # Initialize the time counter
    
    # Loop through the queue until the person at position k is done buying tickets
    while tickets[k] > 0:
        for i in range(len(tickets)):
            # Check if the current person still needs tickets
            if tickets[i] > 0:
                # Decrement their ticket count
                tickets[i] -= 1
                # Increment the time counter
                time += 1
            
            # Break early if the target person (k-th person) is done buying their tickets
            if tickets[k] == 0:
                return time

    return time
```

### Complexity Analysis
- **Time Complexity:** O(n * T), where `n` is the number of people in line, and `T` is the maximum number of tickets any person needs. This is because, in the worst case, we're decrementing the ticket needs up to `T` times across all `n` people.
- **Space Complexity:** O(1), as we are modifying the input in place and not using any extra space proportional to the input size.

This is a straightforward simulation approach that mimics the queue dynamics, ensuring the correct order of ticket purchasing and accurately measuring the time required for the k-th person to complete their buying process.

