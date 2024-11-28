# [950. Reveal Cards In Increasing Order](https://leetcode.com/problems/reveal-cards-in-increasing-order/)

## Approach 1: Simulation Using Deque

### Solution
```java
// Time Complexity: O(n log n)
// Space Complexity: O(n)
import java.util.*;

public class Solution {
    public int[] deckRevealedIncreasing(int[] deck) {
        int n = deck.length;
        int[] result = new int[n];
        Arrays.sort(deck); // Sort the deck in increasing order

        Deque<Integer> deque = new LinkedList<>();

        // Add indices to the deque in order
        for (int i = 0; i < n; i++) {
            deque.add(i);
        }

        // Place the smallest cards in the positions determined by deque
        for (int card : deck) {
            result[deque.pollFirst()] = card; // Place the current smallest card
            if (!deque.isEmpty()) {
                deque.add(deque.pollFirst()); // Move the next index to the back
            }
        }

        return result;
    }
}
```

## Approach 2: Simulation Using Queue

### Solution
```java
// Time Complexity: O(n log n)
// Space Complexity: O(n)
import java.util.*;

public class Solution {
    public int[] deckRevealedIncreasing(int[] deck) {
        Arrays.sort(deck); // Sort the deck in increasing order
        Queue<Integer> queue = new LinkedList<>();

        // Add indices to the queue in order
        for (int i = 0; i < deck.length; i++) {
            queue.add(i);
        }

        int[] result = new int[deck.length];

        // Place the smallest cards in the positions determined by the queue
        for (int card : deck) {
            result[queue.poll()] = card; // Place the current smallest card
            if (!queue.isEmpty()) {
                queue.add(queue.poll()); // Move the next index to the back
            }
        }

        return result;
    }
}
```