# 1472. [Design Browser History](https://leetcode.com/problems/design-browser-history/)

## Approach: Doubly Linked List

### Solution
csharp
```csharp
// Time Complexity:
//   - Visit: O(1)
//   - Back: O(steps)
//   - Forward: O(steps)
// Space Complexity: O(n), where n is the number of visited pages
public class BrowserHistory {
    private class Node {
        public string Url;
        public Node Prev, Next;

        public Node(string url) {
            Url = url;
        }
    }

    private Node current;

    public BrowserHistory(string homepage) {
        current = new Node(homepage);
    }

    public void Visit(string url) {
        Node newNode = new Node(url);
        current.Next = newNode;
        newNode.Prev = current;
        current = newNode; // Move to the newly visited page
    }

    public string Back(int steps) {
        while (steps > 0 && current.Prev != null) {
            current = current.Prev;
            steps--;
        }
        return current.Url;
    }

    public string Forward(int steps) {
        while (steps > 0 && current.Next != null) {
            current = current.Next;
            steps--;
        }
        return current.Url;
    }
}

/**
 * Your BrowserHistory object will be instantiated and called as such:
 * BrowserHistory obj = new BrowserHistory(homepage);
 * obj.Visit(url);
 * string param_2 = obj.Back(steps);
 * string param_3 = obj.Forward(steps);
 */
```

