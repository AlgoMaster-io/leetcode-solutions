# [950. Reveal Cards In Increasing Order](https://leetcode.com/problems/reveal-cards-in-increasing-order/)

## Approach 1: Simulation Using Deque

### Solution
```cpp
// Time Complexity: O(n log n)
// Space Complexity: O(n)
#include <vector>
#include <deque>
#include <algorithm>

class Solution {
public:
    std::vector<int> deckRevealedIncreasing(std::vector<int>& deck) {
        int n = deck.size();
        std::vector<int> result(n);
        std::sort(deck.begin(), deck.end()); // Sort the deck in increasing order

        std::deque<int> deque;

        // Add indices to the deque in order
        for (int i = 0; i < n; i++) {
            deque.push_back(i);
        }

        // Place the smallest cards in the positions determined by deque
        for (int card : deck) {
            result[deque.front()] = card; // Place the current smallest card
            deque.pop_front();
            if (!deque.empty()) {
                deque.push_back(deque.front()); // Move the next index to the back
                deque.pop_front();
            }
        }

        return result;
    }
};
```

## Approach 2: Simulation Using Queue

### Solution
```cpp
// Time Complexity: O(n log n)
// Space Complexity: O(n)
#include <vector>
#include <queue>
#include <algorithm>

class Solution {
public:
    std::vector<int> deckRevealedIncreasing(std::vector<int>& deck) {
        std::sort(deck.begin(), deck.end()); // Sort the deck in increasing order
        std::queue<int> queue;

        // Add indices to the queue in order
        for (int i = 0; i < deck.size(); i++) {
            queue.push(i);
        }

        std::vector<int> result(deck.size());

        // Place the smallest cards in the positions determined by the queue
        for (int card : deck) {
            result[queue.front()] = card; // Place the current smallest card
            queue.pop();
            if (!queue.empty()) {
                queue.push(queue.front()); // Move the next index to the back
                queue.pop();
            }
        }

        return result;
    }
};
```

