# [Leetcode 950: Reveal Cards In Increasing Order](https://leetcode.com/problems/reveal-cards-in-increasing-order/)

## Navigation
- [Approach 1: Simulation with Sorting and Queue](#approach-1)
- [Approach 2: Optimized Simulation Using Sorting and Queue](#approach-2)

## Approach 1: Simulation with Sorting and Queue

### Intuition

The problem requires us to reveal the cards such that they are in increasing order. To achieve this, we can simulate the revealing process by:
1. Starting with a sorted array of the card values.
2. Simulating the deck as a queue where we can keep track of the order of the indices based on the card position.

The plan is:
- Sort the cards to maintain the increasing order.
- Use a queue to manage the order in which we should place the sorted cards in a new result array.

### Steps
1. Sort the cards in ascending order.
2. Initialize a queue with the card indices from 0 to n-1, representing the positions of the deck.
3. For each card (in sorted order), place it in the result array at the position of the front of the queue, and remove that index from the queue.
4. To simulate the reveal process, move the next position (front of the queue) to the back of the queue.

### Code

```typescript
function deckRevealedIncreasing(deck: number[]): number[] {
    // Sort the deck in increasing order
    deck.sort((a, b) => a - b);

    // Create a queue to simulate the process of revealing cards
    const queue: number[] = [];
    for (let i = 0; i < deck.length; i++) {
        queue.push(i);
    }

    // Create a result array to store the order of revealed cards
    const result: number[] = new Array(deck.length);

    // Process the sorted cards in order
    for (const card of deck) {
        // Place the current card at the index given by the front of the queue
        result[queue.shift()!] = card;

        // Move the index at the front of queue to the back to simulate reveal order
        if (queue.length > 0) {
            queue.push(queue.shift()!);
        }
    }

    return result;
}
```

### Complexity Analysis
- **Time Complexity**: O(n log n) due to sorting the deck.
- **Space Complexity**: O(n) for the queue and result arrays.

## Approach 2: Optimized Simulation Using Sorting and Queue

### Intuition

While the basic approach efficiently finds the correct card order, we can refine it by reducing the number of operations with the queue. Instead of re-using a queue to rotate positions, we can handle multiple operations inside the loop more explicitly, which can be more efficient.

### Steps
1. Sort the cards in increasing order.
2. Use a queue but simulate by manipulating it manually inside the loop without needing to rotate elements outside the card handling loop.

### Code

```typescript
function deckRevealedIncreasing(deck: number[]): number[] {
    deck.sort((a, b) => a - b);
    const n = deck.length;
    const queue: number[] = Array.from({ length: n }, (_, i) => i);
    const result: number[] = new Array(n);

    for (let i = 0; i < n; i++) {
        result[queue.shift()!] = deck[i];
        if (i < n - 1) queue.push(queue.shift()!);
    }

    return result;
}
```

### Complexity Analysis
- **Time Complexity**: O(n log n) due to sorting the deck.
- **Space Complexity**: O(n) for queue and result array.

The optimized simulation improves the performance slightly by eliminating some redundant operations and can be particularly beneficial for larger inputs due to its streamlined loop processing.

