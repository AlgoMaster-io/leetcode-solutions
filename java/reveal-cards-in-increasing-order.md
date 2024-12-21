[Leetcode Problem 950: Reveal Cards In Increasing Order](https://leetcode.com/problems/reveal-cards-in-increasing-order/)

## Approaches:
1. [Simple Simulation](#1-simple-simulation)
2. [Optimized Approach Using Queue](#2-optimized-approach-using-queue)

### 1. Simple Simulation

**Intuition:**

The core idea is to simulate the process as described in the problem:
- Sort the deck to ensure that our cards are in increasing order.
- Use an additional array to reconstruct the sequence by following the steps outlined: 
  - Take the top card and put it into a new deck.
  - Move the next card to the bottom of the deck.
- This approach is straightforward but lacks efficiency due to manual simulation.

**Java Code:**

```java
import java.util.Arrays;
import java.util.LinkedList;

public class Solution {
    public int[] deckRevealedIncreasing(int[] deck) {
        // Sort the initial deck to process smallest to largest
        Arrays.sort(deck);
        int n = deck.length;
        LinkedList<Integer> index = new LinkedList<>();
        // Create a list of all indices in the deck
        for (int i = 0; i < n; i++) {
            index.add(i);
        }

        int[] result = new int[n];
        // Simulate the process
        for (int card : deck) {
            // Place the top card in the next available position
            result[index.poll()] = card;
            // Move the next topmost index to the bottom of the queue
            if (!index.isEmpty()) {
                index.add(index.poll());
            }
        }
        return result;
    }
}
```

**Time Complexity:** O(N log N) due to sorting where N is the number of cards.

**Space Complexity:** O(N) for the auxiliary `index` list.

### 2. Optimized Approach Using Queue

**Intuition:**

The improved approach builds upon the simple simulation by efficiently managing indices using a queue:
- First, the deck is sorted, so cards are processed from smallest to largest.
- Use a queue to track the order of indices based on the revelation process.
  - This step mimics the effect of moving cards to the bottom after revealing.
- Place each card in the correct position using indices from the queue in a single pass.

**Java Code:**

```java
import java.util.Arrays;
import java.util.LinkedList;
import java.util.Queue;

public class Solution {
    public int[] deckRevealedIncreasing(int[] deck) {
        // Sort the deck so we can place cards from smallest to largest
        Arrays.sort(deck);
        int n = deck.length;
        Queue<Integer> indexQueue = new LinkedList<>();
        // Create a queue that holds indices from 0 to n-1
        for (int i = 0; i < n; i++) {
            indexQueue.add(i);
        }

        int[] result = new int[n];
        // For each card in the sorted deck, place it at the first index, then rotate the queue
        for (int card : deck) {
            // Place the current smallest card at the index popped from the queue
            result[indexQueue.poll()] = card;
            // Simulate rotation by moving the next index to the bottom of the queue
            if (!indexQueue.isEmpty()) {
                indexQueue.add(indexQueue.poll());
            }
        }
        return result;
    }
}
```

**Time Complexity:** O(N log N) for sorting, and O(N) for queue operations, resulting in overall O(N log N).

**Space Complexity:** O(N) for maintaining the queue. 

This approach provides an efficient and clear method to simulate the card revelation process using a queue structure. It’s more optimized for handling larger decks than manual simulation.

