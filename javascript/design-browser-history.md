# [Leetcode Problem 1472: Design Browser History](https://leetcode.com/problems/design-browser-history/)

## Approaches
- [Approach 1: List with Navigation Index](#approach-1-list-with-navigation-index)
- [Approach 2: Doubly Linked List](#approach-2-doubly-linked-list)

### Approach 1: List with Navigation Index

#### Intuition
In this approach, we can use an array to store the history of the pages one visits with a current pointer that keeps track of the current page in the history. We can append new pages as we navigate, and move the current pointer forward or backward to simulate going back or forward in the history.

#### Solution

```javascript
class BrowserHistory {
  constructor(homepage) {
    // Initialize an array to store the history of visited pages
    this.history = [homepage];
    // Initialize the current index to track position in history
    this.currentIndex = 0;
  }

  visit(url) {
    // Remove all forward history when visiting new page
    this.history = this.history.slice(0, this.currentIndex + 1);
    // Add new page to the history
    this.history.push(url);
    // Update the current index to the new page
    this.currentIndex++;
  }

  back(steps) {
    // Move steps back but stay within bounds of history array
    this.currentIndex = Math.max(0, this.currentIndex - steps);
    // Return the current page
    return this.history[this.currentIndex];
  }

  forward(steps) {
    // Move steps forward but stay within bounds of history array
    this.currentIndex = Math.min(this.history.length - 1, this.currentIndex + steps);
    // Return the current page
    return this.history[this.currentIndex];
  }
}
```

#### Time Complexity
- `visit`: O(1) for slicing and appending.
- `back` / `forward`: O(1) for adjusting index and accessing array.
  
#### Space Complexity
- O(n) for storing history where `n` is the number of `visit` operations.

---

### Approach 2: Doubly Linked List

#### Intuition
Another approach is to use a doubly linked list where each node contains the page URL and pointers to the previous and next pages. This way we can efficiently move back and forth through the history, and erase the forward history in constant time.

#### Solution

```javascript
class ListNode {
  constructor(url) {
    this.url = url;
    this.prev = null;
    this.next = null;
  }
}

class BrowserHistory {
  constructor(homepage) {
    // Initialize a doubly linked list node for homepage
    const node = new ListNode(homepage);
    // Current node set to homepage
    this.current = node;
  }

  visit(url) {
    // Create a new node for the new page
    const newNode = new ListNode(url);
    // Connect the current node to new node and vice versa
    this.current.next = newNode;
    newNode.prev = this.current;
    // Move current to the new node
    this.current = newNode;
    // Since we visited a new site, forward history is erased
    this.current.next = null;
  }

  back(steps) {
    // Move steps back but stay within the bounds of the list
    while (this.current.prev !== null && steps > 0) {
      this.current = this.current.prev;
      steps--;
    }
    return this.current.url;
  }

  forward(steps) {
    // Move steps forward but stay within the bounds of the list
    while (this.current.next !== null && steps > 0) {
      this.current = this.current.next;
      steps--;
    }
    return this.current.url;
  }
}
```

#### Time Complexity
- `visit`: O(1) for adding a page.
- `back` / `forward`: O(1) per step, potentially O(n) for moving across history.

#### Space Complexity
- O(n) for storing history nodes where `n` is the number of `visit` operations.

Each approach offers efficient random access to browser history operations, with the doubly linked list being slightly more aligned with typical implementations of history tracking in software with continuous backward/forward iterations.

