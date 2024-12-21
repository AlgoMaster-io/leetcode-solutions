# [950. Reveal Cards In Increasing Order](https://leetcode.com/problems/reveal-cards-in-increasing-order/)

## Approaches
- [Approach 1: Simulation](#approach-1-simulation)
- [Approach 2: Optimal Queue Simulation](#approach-2-optimal-queue-simulation)

### Approach 1: Simulation

#### Intuition
The main idea behind the problem is to simulate the process of revealing cards. In order to sort the cards in increasing order while respecting the reveal pattern, we can:
1. Sort the given deck.
2. Simulate the reveal process using a queue data structure.

We'll use a queue to simulate the order of revealing. Initially, the queue would represent the indices of the resulting array in the initial order of cards being revealed, which will undergo modifications as per the simulation process.

#### Steps
1. Sort the given deck.
2. Initialize a queue that contains the index positions in order from 0 to n-1.
3. Create an answer array to store the sorted deck in the reveal order.
4. Simulate the process:
    - Assign the first sorted card to the first index from the queue.
    - Move the next index to the end of the queue to simulate the "put it at the bottom" operation.
5. Continue until all indices have been filled in the result array.

```go
func deckRevealedIncreasing(deck []int) []int {
    n := len(deck)
    sort.Ints(deck)
    result := make([]int, n)
    
    // Initialize the queue of indices
    queue := make([]int, n)
    for i := 0; i < n; i++ {
        queue[i] = i
    }

    // Simulate the process
    for _, card := range deck {
        // Place the smallest available card in the position
        result[queue[0]] = card
        queue = queue[1:]  // Remove the used position
        if len(queue) > 0 {
            // Move the top position to the bottom to simulate the reveal process
            queue = append(queue, queue[0])
            queue = queue[1:]
        }
    }
    
    return result
}
```

**Time Complexity:** O(n log n) due to the sorting step.  
**Space Complexity:** O(n) for the queue and result array.

### Approach 2: Optimal Queue Simulation

#### Intuition
While the first approach uses a simple queue to simulate the process, this can be optimized further by directly building the result in reverse order. 
We can determine the indices where cards should go by repeatedly simulating the "reveal and place to the bottom" process in reverse.

#### Steps
1. Sort the deck.
2. Initialize an empty queue for indices.
3. Start from the largest card, simulate placing it at the top, and perform reverse simulation:
    - If there are already indices, move the last one to the front to simulate in reverse.
    - Place the current largest card in the front of the resulting index queue.

```go
func deckRevealedIncreasing(deck []int) []int {
    sort.Ints(deck)
    n := len(deck)
    indexes := make([]int, 0, n)

    // Prepare the order of indices in reverse
    for i := range deck {
        indexes = append([]int{i}, indexes...)
        
        if len(indexes) > 1 {
            // Move the largest element to the back as the simulation in reverse
            indexes = append([]int{indexes[len(indexes)-1]}, indexes[:len(indexes)-1]...)
        }
    }
    
    result := make([]int, n)
    // Fill result array using the prepared ordering of indices
    for i, idx := range indexes {
        result[idx] = deck[i]
    }

    return result
}
```

**Time Complexity:** O(n log n) due to sorting. Re-ordering indices takes O(n).  
**Space Complexity:** O(n) for storing the result and indices.

By comparing these two approaches, Approach 2 is more optimized in terms of reducing unnecessary operations on the data structure while maintaining the same time complexity.

