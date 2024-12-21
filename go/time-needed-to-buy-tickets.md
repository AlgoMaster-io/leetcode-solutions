# [LeetCode 2073: Time Needed to Buy Tickets](https://leetcode.com/problems/time-needed-to-buy-tickets/)

## Approaches
- [Approach 1: Simulation Using Queue](#approach-1-simulation-using-queue)
- [Approach 2: Optimized Counter Simulation](#approach-2-optimized-counter-simulation)

### Approach 1: Simulation Using Queue

**Intuition:**

The problem revolves around simulating the purchasing of tickets in a queue system while considering each person can only buy one ticket in a single second. We can simulate this by maintaining a circular queue and decrease the ticket count of the person at the front by one until the desired person completes their ticket buying.

**Detailed Steps:**

1. Use a loop to simulate each second of the queue operation.
2. Decrease the ticket count for the front-most person.
3. If the person still has tickets remaining, move them to the back of the queue.
4. If the person has completed their transaction (i.e., has 0 tickets left) and it is the desired person, end the simulation and return the time.
5. Repeat until the desired person finishes buying their tickets.

**Code:**

```go
func timeRequiredToBuy(tickets []int, k int) int {
    time := 0
    n := len(tickets)
    
    for {
        for i := 0; i < n; i++ {
            if tickets[i] > 0 { // if there are tickets to buy
                tickets[i]-- // buy a ticket
                time++
                if i == k && tickets[i] == 0 { // if target person has finished
                    return time
                }
            }
        }
    }
}
```

**Complexity:**

- **Time Complexity:** \(O(n \times t)\), where \(n\) is the number of people and \(t\) is the maximum number of tickets any person needs to buy. We process each person repeatedly until everyone's tickets are bought.
- **Space Complexity:** \(O(1)\), only time variable impacts extra space.

### Approach 2: Optimized Counter Simulation

**Intuition:**

Instead of simulating each second, we simply calculate the total time required. Each person before the desired index will contribute time equivalent to the minimum of their tickets and the target's tickets (since once the target finishes, no further contributions from others are needed). For the target and those after, we only count what they need until the target finishes.

**Detailed Steps:**

1. Initialize total time as zero.
2. Iterate through each person and calculate how many tickets each person needs to buy up to the minimum of k’s tickets when that person has not been processed yet. 
3. For all persons after k, just add their full amount if target person does not exceed it.
4. Sum these ticket requirements into a total time.

**Code:**

```go
func timeRequiredToBuy(tickets []int, k int) int {
    time := 0
    for i := 0; i < len(tickets); i++ {
        if i <= k {
            time += min(tickets[i], tickets[k]) // account target's tickets with respect to previous
        } else {
            time += min(tickets[i], tickets[k]-1) // after k, consider just until target finishes
        }
    }
    return time
}

// Helper function to get the minimum of two integers
func min(a, b int) int {
    if a < b {
        return a
    }
    return b
}
```

**Complexity:**

- **Time Complexity:** \(O(n)\), as we only need to pass through the list once.
- **Space Complexity:** \(O(1)\), no extra space used beyond predefined variables.

By considering all cases and simulating favorable scenarios progressively or in a single pass, we optimize calculations and provide an efficient solution.

