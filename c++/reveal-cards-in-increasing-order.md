# [Leetcode Problem 950: Reveal Cards In Increasing Order](https://leetcode.com/problems/reveal-cards-in-increasing-order/)

## Approaches:
1. [Simulate Deck Order Using a Queue](#approach-1-simulate-deck-order-using-a-queue)
2. [Optimal Simulation using Deque](#approach-2-optimal-simulation-using-deque)

---

## Approach 1: Simulate Deck Order Using a Queue

### Intuition:
To understand the problem, imagine the process: We need to rearrange such that revealing them one by one in the deck order results in cards appearing in increasing order. We start with a sorted deck and simulate the revealing process using a queue to help track positions.

- Sort the initial deck of cards.
- Use a queue to simulate exposing the cards process.

**Steps:**
1. Sort the given deck.
2. Initialize an empty queue `q` to keep track of indices where the sorted cards should be placed.
3. Push all indices of the deck into the queue initially.
4. Iterate over each card in sorted deck:
    - Assign the card at the index given by the front of the queue.
    - Pop this index, and then push it back to simulate the deck process one move.
5. The result deck will reflect the increasing order when revealed turn-by-turn.

### Code:
```cpp
#include <vector>
#include <algorithm>
#include <queue>
using namespace std;

vector<int> deckRevealedIncreasing(vector<int>& deck) {
    sort(deck.begin(), deck.end()); // Sort the deck

    int n = deck.size();
    queue<int> q;
    
    for (int i = 0; i < n; ++i) {
        q.push(i); // Initialize the queue with all index positions
    }
    
    vector<int> result(n);
    
    for (int card : deck) {
        result[q.front()] = card; // Place the card at the correct position
        q.pop(); // Remove the index from the queue
        
        if (!q.empty()) {
            q.push(q.front()); // Move the next index to the bottom of the queue
            q.pop(); // as per the reveal card process
        }
    }
    
    return result;
}
```

### Time and Space Complexity:
- **Time Complexity:** \(O(n \log n)\) due to sorting the deck.
- **Space Complexity:** \(O(n)\) for the queue used to store indices.

---

## Approach 2: Optimal Simulation using Deque

### Intuition:
While the queue above works well, we can optimize by recognizing that indices accessed last are immediately reused, making a deque a perfect fit for this modification.

- Sort the deck.
- Use a deque to simulate exposure, leveraging its ability to add and remove elements from both ends.

**Steps:**
1. Sort the input deck.
2. Initialize a deque.
3. Iterate over the sorted deck from the largest to the smallest:
    - For each card, place it in the front of the deque.
    - Simulate revealing by rotating the deck order (achieved by moving the last element to the front).

### Code:
```cpp
#include <vector>
#include <algorithm>
#include <deque>
using namespace std;

vector<int> deckRevealedIncreasing(vector<int>& deck) {
    sort(deck.begin(), deck.end());
    int n = deck.size();
    deque<int> dq;

    for (int i = n - 1; i >= 0; --i) {
        if (!dq.empty()) {
            dq.push_front(dq.back()); // Simulate reveal by placing last to first
            dq.pop_back();
        }
        dq.push_front(deck[i]); // Place current card at the start
    }
    
    return vector<int>(dq.begin(), dq.end());
}
```

### Time and Space Complexity:
- **Time Complexity:** \(O(n \log n)\) for sorting.
- **Space Complexity:** \(O(n)\) for the deque used to store indices temporarily.

This approach utilizes deque operations effectively, allowing us to achieve the simulation in a way that mimics the natural reveal order swiftly.

