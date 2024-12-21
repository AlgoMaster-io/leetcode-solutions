# [Leetcode 1472: Design Browser History](https://leetcode.com/problems/design-browser-history/)

## Table of Contents

- [Approach 1: Simple Array with Pointer](#approach-1-simple-array-with-pointer)
- [Approach 2: Doubly Linked List](#approach-2-doubly-linked-list)

### Approach 1: Simple Array with Pointer

#### Intuition:
The simplest way to solve the problem of designing a browser history is to use an array to keep track of the history of pages visited and use a pointer that represents the current page. This approach is straightforward but has limitations regarding space efficiency and operations when you need to discard future page visits.

#### Steps:
- Use an array `history` to store the visited URLs.
- Use an integer `current` to keep track of the index of the current page.
- On visiting a new page, append it to the array and update the `current` index. If `current` is not at the end of the array, truncate the array at the `current` index before appending.
- For moving back, decrement the `current` index, without going below 0.
- For moving forward, increment the `current` index, without exceeding the bounds of the array.

#### Code:

```typescript
class BrowserHistory {
    private history: string[];
    private current: number;

    constructor(homepage: string) {
        this.history = [homepage];
        this.current = 0;
    }

    visit(url: string): void {
        // Truncate the history at the current position,
        // as we are visiting a new page and all the forward history should be discarded.
        this.history = this.history.slice(0, this.current + 1);
        this.history.push(url);
        this.current++;
    }

    back(steps: number): string {
        // Move the current pointer back by steps, making sure it stays within bounds.
        this.current = Math.max(0, this.current - steps);
        return this.history[this.current];
    }

    forward(steps: number): string {
        // Move the current pointer forward by steps, making sure it stays within bounds.
        this.current = Math.min(this.history.length - 1, this.current + steps);
        return this.history[this.current];
    }
}
```

#### Complexity Analysis:
- **Time Complexity**: 
  - `visit`: O(n) in the worst case, due to the array truncation.
  - `back` and `forward`: O(1), since we're only adjusting the index.
- **Space Complexity**: O(n), where n is the number of pages stored in history.

### Approach 2: Doubly Linked List

#### Intuition:
Using a doubly linked list can be more efficient since it allows us to manage our history without needing to shift or truncate arrays. Each node in the list could represent a page, with pointers to both the previous and next pages. This approach optimizes insertions and deletions at the cost of more complex node management.

#### Steps:
- Define a `Node` class for the doubly linked list that holds a URL and pointers to the previous and next nodes.
- Maintain a reference to the current node, initialized to the homepage node.
- On a new visit, create a new node and adjust pointers accordingly, invalidating any nodes that were previously forward of the current node.
- For back and forward navigations, move the current node to the previous or next node, respecting list bounds.

#### Code:

```typescript
class Node {
    url: string;
    prev: Node | null;
    next: Node | null;

    constructor(url: string) {
        this.url = url;
        this.prev = null;
        this.next = null;
    }
}

class BrowserHistory {
    private current: Node;

    constructor(homepage: string) {
        this.current = new Node(homepage);
    }

    visit(url: string): void {
        const newNode = new Node(url);
        // Link the new node with the current one:
        newNode.prev = this.current;
        this.current.next = newNode;
        // Move the current pointer to the new node:
        this.current = newNode;
    }

    back(steps: number): string {
        while (steps > 0 && this.current.prev !== null) {
            this.current = this.current.prev;
            steps--;
        }
        return this.current.url;
    }

    forward(steps: number): string {
        while (steps > 0 && this.current.next !== null) {
            this.current = this.current.next;
            steps--;
        }
        return this.current.url;
    }
}
```

#### Complexity Analysis:
- **Time Complexity**: 
  - `visit`, `back`, and `forward`: O(steps), where steps is the number of steps moved.
- **Space Complexity**: O(n), where n is the number of unique pages a user has navigated through, since each page is stored in a node of the linked list.

