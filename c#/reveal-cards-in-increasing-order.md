[Leetcode Problem 950: Reveal Cards In Increasing Order](https://leetcode.com/problems/reveal-cards-in-increasing-order/)

## Approaches
- [Approach 1: Simulation with Queue](#approach-1-simulation-with-queue)
- [Approach 2: Optimized Simulation with Deque](#approach-2-optimized-simulation-with-deque)

### Approach 1: Simulation with Queue

**Intuition:**
1. Sort the deck so the smallest card is on top.
2. Use a queue to simulate the process of revealing cards and placing them at the back.
3. Initially, enqueue all indices (0 to n-1) of the result array.
4. Reveal the smallest card from the sorted array to the position in the result indicated by the front of the queue.
5. Move this index to the back of the queue and repeat for each card in the deck.

**Algorithm:**
1. Sort the input deck.
2. Initialize a queue with indices from 0 to n-1.
3. Create a result array to store the final order.
4. For each card in the sorted deck:
   - Place the card in the result array at the index at the front of the queue and dequeue this index.
   - Move that index to the back of the queue if there are indices left.

**Code:**
```csharp
public int[] DeckRevealedIncreasing(int[] deck) {
    // Sort the deck in increasing order
    Array.Sort(deck);
    int n = deck.Length;
    
    // Initialize a queue to handle the simulation
    Queue<int> indexQueue = new Queue<int>();
    for (int i = 0; i < n; i++) {
        indexQueue.Enqueue(i);
    }
    
    // Result array
    int[] result = new int[n];
    
    // Simulate the process
    foreach (int card in deck) {
        // Place the current card at the current index
        int index = indexQueue.Dequeue();
        result[index] = card;
        
        // Move the next index to the back of the queue if there's any left
        if (indexQueue.Count > 0) {
            indexQueue.Enqueue(indexQueue.Dequeue());
        }
    }
    
    return result;
}
```

**Complexity Analysis:**
- Time Complexity: O(n log n), where n is the number of cards, dominated by the sorting step.
- Space Complexity: O(n) for the queue and result array.

### Approach 2: Optimized Simulation with Deque

**Intuition:**
1. Use a deque (double-ended queue) for better performance when rearranging indices compared to a single-ended queue.
2. This allows us to efficiently append and remove items from both ends, providing us with a mechanism to simulate the problem more naturally.

**Algorithm:**
1. Sort the deck.
2. Initialize a deque with indices from 0 to n-1.
3. Create a result array for the final order.
4. For each card in the sorted deck:
   - Pop the leftmost index from the deque and assign the card to the result array at this position.
   - Move the next leftmost index to the right end of the deque.

**Code:**
```csharp
public int[] DeckRevealedIncreasing(int[] deck) {
    // Sort the deck numbers
    Array.Sort(deck);
    int n = deck.Length;
    
    // Use a deque to keep track of indices
    LinkedList<int> indexDeque = new LinkedList<int>();
    for (int i = 0; i < n; i++) {
        indexDeque.AddLast(i);
    }
    
    // Final result array that will be filled
    int[] result = new int[n];

    // Iterate through the sorted deck
    foreach (int card in deck) {
        // Place the card at the position of the first index in the deque
        int index = indexDeque.First.Value;
        indexDeque.RemoveFirst();
        result[index] = card;
        
        // If there are still indices, move the next index to the back
        if (indexDeque.Count > 0) {
            indexDeque.AddLast(indexDeque.First.Value);
            indexDeque.RemoveFirst();
        }
    }

    return result;
}
```

**Complexity Analysis:**
- Time Complexity: O(n log n), for sorting the deck.
- Space Complexity: O(n) to store the deque and result array.

