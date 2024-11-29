# 1472. [Design Browser History](https://leetcode.com/problems/design-browser-history/)

## Approach: Doubly Linked List

### Solution
```javascript
// Time Complexity:
//   - visit: O(1)
//   - back: O(steps)
//   - forward: O(steps)
// Space Complexity: O(n), where n is the number of visited pages
class BrowserHistory {
    constructor(homepage) {
        this.current = new Node(homepage);
    }

    visit(url) {
        const newNode = new Node(url);
        this.current.next = newNode;
        newNode.prev = this.current;
        this.current = newNode; // Move to the newly visited page
    }

    back(steps) {
        while (steps > 0 && this.current.prev !== null) {
            this.current = this.current.prev;
            steps--;
        }
        return this.current.url;
    }

    forward(steps) {
        while (steps > 0 && this.current.next !== null) {
            this.current = this.current.next;
            steps--;
        }
        return this.current.url;
    }
}

class Node {
    constructor(url) {
        this.url = url;
        this.prev = null;
        this.next = null;
    }
}

/**
 * Your BrowserHistory object will be instantiated and called as such:
 * const obj = new BrowserHistory(homepage);
 * obj.visit(url);
 * const param_2 = obj.back(steps);
 * const param_3 = obj.forward(steps);
 */
```

