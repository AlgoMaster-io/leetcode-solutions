# [950. Reveal Cards In Increasing Order](https://leetcode.com/problems/reveal-cards-in-increasing-order/)

## Approach 1: Simulation Using Deque

### Solution
typescript
```typescript
// Time Complexity: O(n log n)
// Space Complexity: O(n)

function deckRevealedIncreasing(deck: number[]): number[] {
    const n = deck.length;
    const result: number[] = new Array(n);
    deck.sort((a, b) => a - b); // Sort the deck in increasing order

    const deque: number[] = [];

    // Add indices to the deque in order
    for (let i = 0; i < n; i++) {
        deque.push(i);
    }

    // Place the smallest cards in the positions determined by deque
    for (const card of deck) {
        result[deque.shift()!] = card; // Place the current smallest card
        if (deque.length > 0) {
            deque.push(deque.shift()!); // Move the next index to the back
        }
    }

    return result;
}
```

## Approach 2: Simulation Using Queue

### Solution
typescript
```typescript
// Time Complexity: O(n log n)
// Space Complexity: O(n)

function deckRevealedIncreasing(deck: number[]): number[] {
    deck.sort((a, b) => a - b); // Sort the deck in increasing order
    const queue: number[] = [];

    // Add indices to the queue in order
    for (let i = 0; i < deck.length; i++) {
        queue.push(i);
    }

    const result: number[] = new Array(deck.length);

    // Place the smallest cards in the positions determined by the queue
    for (const card of deck) {
        result[queue.shift()!] = card; // Place the current smallest card
        if (queue.length > 0) {
            queue.push(queue.shift()!); // Move the next index to the back
        }
    }

    return result;
}
```

