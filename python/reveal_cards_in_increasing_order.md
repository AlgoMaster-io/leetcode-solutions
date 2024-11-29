# [950. Reveal Cards In Increasing Order](https://leetcode.com/problems/reveal-cards-in-increasing-order/)

## Approach 1: Simulation Using Deque

### Solution
```python
# Time Complexity: O(n log n)
# Space Complexity: O(n)
from collections import deque

class Solution:
    def deckRevealedIncreasing(self, deck):
        n = len(deck)
        result = [0] * n
        deck.sort()  # Sort the deck in increasing order

        deque_indices = deque(range(n))

        # Place the smallest cards in the positions determined by deque
        for card in deck:
            result[deque_indices.popleft()] = card  # Place the current smallest card
            if deque_indices:
                deque_indices.append(deque_indices.popleft())  # Move the next index to the back

        return result
```

## Approach 2: Simulation Using Queue

### Solution
```python
# Time Complexity: O(n log n)
# Space Complexity: O(n)
from collections import deque

class Solution:
    def deckRevealedIncreasing(self, deck):
        deck.sort()  # Sort the deck in increasing order
        queue_indices = deque(range(len(deck)))

        result = [0] * len(deck)

        # Place the smallest cards in the positions determined by the queue
        for card in deck:
            result[queue_indices.popleft()] = card  # Place the current smallest card
            if queue_indices:
                queue_indices.append(queue_indices.popleft())  # Move the next index to the back

        return result
```

