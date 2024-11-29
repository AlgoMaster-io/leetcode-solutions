# [2073. Time Needed to Buy Tickets](https://leetcode.com/problems/time-needed-to-buy-tickets/)

## Approach 1: Simulation with a Loop

### Solution
go
```go
// Time Complexity: O(n * max(tickets))
// Space Complexity: O(1)
package main

func timeRequiredToBuy(tickets []int, k int) int {
    time := 0

    // Simulate the ticket-buying process
    for tickets[k] > 0 {
        for i := 0; i < len(tickets); i++ {
            if tickets[i] > 0 {
                tickets[i]-- // Person buys one ticket
                time++       // Increment time for each ticket bought
            }
            // Stop as soon as the person at index k finishes buying
            if tickets[k] == 0 {
                return time
            }
        }
    }

    return time
}
```

## Approach 2: Optimized Calculation

### Solution
go
```go
// Time Complexity: O(n)
// Space Complexity: O(1)
package main

func timeRequiredToBuy(tickets []int, k int) int {
    time := 0

    // Iterate through all the people in line
    for i := 0; i < len(tickets); i++ {
        // Add the minimum tickets this person can buy
        if i <= k {
            time += min(tickets[i], tickets[k])
        } else {
            time += min(tickets[i], tickets[k]-1)
        }
    }

    return time
}

// Helper function to find the minimum of two integers
func min(a, b int) int {
    if a < b {
        return a
    }
    return b
}
```

