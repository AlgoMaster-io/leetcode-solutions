## [Leetcode 950: Reveal Cards In Increasing Order](https://leetcode.com/problems/reveal-cards-in-increasing-order/)

### Approaches:
1. [Simulation with Queue Approach](#simulation-with-queue-approach)
2. [Optimal Simulation with Deque](#optimal-simulation-with-deque)

---

### Simulation with Queue Approach

#### Intuition:
The problem requires simulating the revealing process, which involves sorting the deck and then placing the cards in a specific order to simulate the described card unveiling. A straightforward way is to mimic the process with a queue, where the front card is placed in the result deck, and the next card is moved to the back of the queue.

#### Steps:
1. Sort the deck so that we can easily determine which cards will appear in the sorted order.
2. Use a queue to simulate the process:
   - Initially, fill the queue with indices from 0 to n-1.
   - For each sorted card, take the index from the front of the queue to place the card in the result.
   - Move the next index in the queue to the back to simulate the card hiding.

#### Code:
```python
from collections import deque

def deckRevealedIncreasing(deck):
    # Sort the given deck of cards
    deck.sort()
    
    # Create an index queue filled with indices from 0 to n-1
    index_queue = deque(range(len(deck)))
    
    # Create a result array with the same length as the deck
    result = [0] * len(deck)
    
    for card in deck:
        # Get the index from the front of the queue
        result[index_queue.popleft()] = card
        
        # If there are indices left, simulate the hiding
        if index_queue:
            index_queue.append(index_queue.popleft())
    
    return result
```

#### Time Complexity:
- Sorting the deck takes `O(n log n)` time.
- Filling the result array takes `O(n)` time.

**Overall Time Complexity:** `O(n log n)`

#### Space Complexity:
- The index queue uses `O(n)` space.
- The result array uses `O(n)` space.

**Overall Space Complexity:** `O(n)`

---

### Optimal Simulation with Deque

#### Intuition:
An optimal approach involves using a deque to simplify the simulation by starting from the back of the sorted deck and using the inherent rotational capabilities of deque to reverse the process elegantly.

#### Steps:
1. Sort the deck.
2. Use a deque to simulate the reverse revealing process:
   - Initially start with an empty deque.
   - For each card in the sorted deck (reverse iterating):
     - Append the card to the front.
     - Rotate the deque if it's not empty to simulate the initial condition.

#### Code:
```python
from collections import deque

def deckRevealedIncreasing(deck):
    # Sort the given deck of cards
    deck.sort()
    
    dq = deque()
    
    for card in reversed(deck):
        # If the deque is not empty, rotate it
        if dq:
            dq.appendleft(dq.pop())
        
        # Add the current card to the front of the deque
        dq.appendleft(card)
    
    return list(dq)
```

#### Time Complexity:
- Sorting the deck takes `O(n log n)` time.
- Filling the deque and rotating is `O(n)` time.

**Overall Time Complexity:** `O(n log n)`

#### Space Complexity:
- The deque uses `O(n)` space.

**Overall Space Complexity:** `O(n)`

---

Both approaches effectively solve the problem, with the latter being slightly more space-efficient due to a more straightforward usage of deque operations.

