# [Leetcode 1472: Design Browser History](https://leetcode.com/problems/design-browser-history/)

## Approaches
- [Approach 1: Naive Solution using List](#approach-1-naive-solution-using-list)
- [Approach 2: Optimized Solution using Doubly-Linked List](#approach-2-optimized-solution-using-doubly-linked-list)

---

## Approach 1: Naive Solution using List

### Intuition
The problem requires designing a system that simulates a browser's backward, forward, and visit operations. A simple way to achieve this is by using a list to store the history of visited URLs and an index to keep track of the current page. The list can be manipulated directly to simulate the behavior of a web browser:

1. **Visit**: Append the URL to the list and move the index to the last position.
2. **Back**: Move the index back by `steps`, ensuring it doesn't go negative.
3. **Forward**: Move the index forward by `steps`, ensuring it doesn't exceed the last position in the list.

### Solution

```python
class BrowserHistory:
    def __init__(self, homepage: str):
        # Initialize history with the homepage.
        self.history = [homepage]
        # Start with the current page index set to 0 pointing to homepage.
        self.current = 0

    def visit(self, url: str) -> None:
        # When visiting a new url, truncate everything beyond the current index.
        self.history = self.history[:self.current + 1]
        # Append the new url to the history.
        self.history.append(url)
        # Move the current index to the new url.
        self.current += 1

    def back(self, steps: int) -> str:
        # Move the index back by 'steps', ensuring not to go below zero.
        self.current = max(0, self.current - steps)
        # Return the current page's url.
        return self.history[self.current]
    
    def forward(self, steps: int) -> str:
        # Move the index forward by 'steps', ensuring not to go beyond the list length.
        self.current = min(len(self.history) - 1, self.current + steps)
        # Return the current page's url.
        return self.history[self.current]
```

### Time Complexity
- `visit`: O(1)
- `back`: O(1)
- `forward`: O(1)

### Space Complexity
- O(n), where n is the number of visited pages.

---

## Approach 2: Optimized Solution using Doubly-Linked List

### Intuition
While the list approach is simple and effective, a doubly-linked list can provide a more robust solution. Doubly-linked lists allow for efficient insertions and deletions, which makes them suitable for this problem because:

1. **Visit**: Append the URL to the current node and move to the new node.
2. **Back**: Move to the previous node if it exists.
3. **Forward**: Move to the next node if it exists.

This approach optimizes memory usage by ensuring we only keep necessary history available.

### Solution

```python
class ListNode:
    def __init__(self, url: str):
        self.url = url
        self.prev = None
        self.next = None

class BrowserHistory:
    def __init__(self, homepage: str):
        # Initialize linked list with homepage.
        self.current = ListNode(homepage)

    def visit(self, url: str) -> None:
        # Create a new node for the new url.
        new_node = ListNode(url)
        # Point current's next to this new node.
        self.current.next = new_node
        # Set new node's previous to current.
        new_node.prev = self.current
        # Move the current to the new node.
        self.current = new_node

    def back(self, steps: int) -> str:
        # Move back as many steps as possible.
        while steps > 0 and self.current.prev:
            self.current = self.current.prev
            steps -= 1
        # Return the current node's url.
        return self.current.url

    def forward(self, steps: int) -> str:
        # Move forward as many steps as possible.
        while steps > 0 and self.current.next:
            self.current = self.current.next
            steps -= 1
        # Return the current node's url.
        return self.current.url
```

### Time Complexity
- `visit`: O(1)
- `back`: O(min(n, steps)), where n is the number of visited pages
- `forward`: O(min(n, steps)), where n is the number of visited pages

### Space Complexity
- O(n), where n is the number of visited pages.

