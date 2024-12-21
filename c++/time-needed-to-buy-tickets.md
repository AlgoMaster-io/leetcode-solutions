## [LeetCode 2073: Time Needed to Buy Tickets](https://leetcode.com/problems/time-needed-to-buy-tickets/)

### Approaches
- [Approach 1: Simulation using Simple Loop - O(N*K) Time Complexity](#approach-1)
- [Approach 2: Optimized Simulation with Queue - O(N) Time Complexity](#approach-2)

---

### Approach 1: Simulation using Simple Loop - O(N*K) Time Complexity

#### Intuition:
The brute force approach involves simulating each person buying tickets. We iterate through the list of people, reducing their ticket count one by one until the person at `k` finishes buying all the tickets they need. This approach is straightforward but inefficient, especially if every person needs a lot of tickets.

#### Steps:
1. Initialize a variable `time` to count the total time elapsed.
2. Use a loop to continuously iterate over the list of people.
3. For every person, if they still need tickets, decrease their ticket count and increment the `time`.
4. Stop when the person at index `k` has no tickets left.
5. The loop ensures the simulation continues in a cyclic manner until the target person `k` finishes buying all their tickets.

```cpp
int timeRequiredToBuy(vector<int>& tickets, int k) {
    int time = 0;
    int n = tickets.size();
    
    // Simulate the ticket buying process
    while (tickets[k] > 0) {
        for (int i = 0; i < n; ++i) {
            if (tickets[i] > 0) { // If the person has tickets to buy
                tickets[i]--;
                time++;
                
                // If k-th person has finished buying tickets
                if (tickets[k] == 0) {
                    return time;
                }
            }
        }
    }
    
    return time;
}
```

- **Time Complexity**: O(N*K) where N is the number of people and K is the maximum number of tickets any person has to buy.
- **Space Complexity**: O(1) since we are not using any extra space except for variables.

---

### Approach 2: Optimized Simulation with Queue - O(N) Time Complexity

#### Intuition:
The optimized approach leverages the fact that the total time needed can be calculated without simulating every single ticket buying event per person. We observe that:
- For everyone before the k-th person, they will buy `min(tickets[i], tickets[k])` tickets.
- For the k-th person, they will buy exactly `tickets[k]` tickets.
- For everyone after the k-th person, they will buy `min(tickets[i], tickets[k] - 1)` because the k-th person finishes with his last ticket before these people get their k-th ticket.

#### Steps:
1. Initialize a variable `time` to accumulate the total time needed.
2. Iterate over the list:
   - Before reaching person `k`, each person contributes `min(tickets[i], tickets[k])` to the total time.
   - Once reaching person `k`, simply add `tickets[k]` to `time`.
   - After person `k`, each person contributes `min(tickets[i], tickets[k] - 1)` to account for stopping before the last round for person `k`.
3. Return the total `time`.

```cpp
int timeRequiredToBuy(vector<int>& tickets, int k) {
    int time = 0;
    int n = tickets.size();
    
    for (int i = 0; i < n; ++i) {
        if (i <= k) {
            // Before or at k, including full tickets[k]
            time += min(tickets[i], tickets[k]);
        } else {
            // After k consideration
            time += min(tickets[i], tickets[k] - 1);
        }
    }
    
    return time;
}
```

- **Time Complexity**: O(N), where N is the number of people.
- **Space Complexity**: O(1) as there are no additional data structures used.

This optimized approach efficiently computes the solution in a single pass over the array, significantly improving performance compared to the naive simulation.

