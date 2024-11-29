# 1472. [Design Browser History](https://leetcode.com/problems/design-browser-history/)

## Approach: Doubly Linked List

### Solution
typescript
```typescript
// Time Complexity:
//   - visit: O(1)
//   - back: O(steps)
//   - forward: O(steps)
// Space Complexity: O(n), where n is the number of visited pages
class BrowserHistory {
    private class Node {
        url: string;
        prev: Node | null = null;
        next: Node | null = null;

        constructor(url: string) {
            this.url = url;
        }
    }

    private current: Node;

    constructor(homepage: string) {
        this.current = new this.Node(homepage);
    }

    visit(url: string): void {
        const newNode = new this.Node(url);
        this.current.next = newNode;
        newNode.prev = this.current;
        this.current = newNode; // Move to the newly visited page
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

/**
 * Your BrowserHistory object will be instantiated and called as such:
 * const obj = new BrowserHistory(homepage);
 * obj.visit(url);
 * const param_2 = obj.back(steps);
 * const param_3 = obj.forward(steps);
 */
```

