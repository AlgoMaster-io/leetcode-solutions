# 1472. [Design Browser History](https://leetcode.com/problems/design-browser-history/)

## Approach: Doubly Linked List

### Solution
```java
// Time Complexity:
//   - visit: O(1)
//   - back: O(steps)
//   - forward: O(steps)
// Space Complexity: O(n), where n is the number of visited pages
class BrowserHistory {
    private class Node {
        String url;
        Node prev, next;

        Node(String url) {
            this.url = url;
        }
    }

    private Node current;

    public BrowserHistory(String homepage) {
        current = new Node(homepage);
    }

    public void visit(String url) {
        Node newNode = new Node(url);
        current.next = newNode;
        newNode.prev = current;
        current = newNode; // Move to the newly visited page
    }

    public String back(int steps) {
        while (steps > 0 && current.prev != null) {
            current = current.prev;
            steps--;
        }
        return current.url;
    }

    public String forward(int steps) {
        while (steps > 0 && current.next != null) {
            current = current.next;
            steps--;
        }
        return current.url;
    }
}

/**
 * Your BrowserHistory object will be instantiated and called as such:
 * BrowserHistory obj = new BrowserHistory(homepage);
 * obj.visit(url);
 * String param_2 = obj.back(steps);
 * String param_3 = obj.forward(steps);
 */
```