## [Leetcode 1472: Design Browser History](https://leetcode.com/problems/design-browser-history/)

### Approaches
- [Approach 1: Naive ArrayList Implementation](#approach-1-naive-arraylist-implementation)
- [Approach 2: Efficient Bidirectional Linked List](#approach-2-efficient-bidirectional-linked-list)

### Approach 1: Naive ArrayList Implementation

#### Intuition
The basic idea is to utilize a dynamic list to simulate the browser's history. For each new visit, simply add the new URL to a list. However, whenever a new URL is visited after going back in history, we must ensure that we delete all forward URLs that become unreachable.

#### Steps
1. Use an `ArrayList` to store the history of URLs.
2. Maintain an integer pointer `currentIndex` to point to the current position in the history.
3. On visiting a new page:
   - Remove all elements in the history after `currentIndex` (since those forward entries are now invalid).
   - Append the new page to the history.
   - Update `currentIndex` to the new position.
4. On back and forward operations, adjust `currentIndex` accordingly but ensure it stays within valid bounds.

```csharp
public class BrowserHistory {
    private List<string> history;
    private int currentIndex;

    public BrowserHistory(string homepage) {
        history = new List<string> { homepage };
        currentIndex = 0;
    }
    
    public void Visit(string url) {
        // Remove all forward entries
        history = history.Take(currentIndex + 1).ToList();
        
        // Add the new page
        history.Add(url);
        
        // Move the current index to the latest page
        currentIndex++;
    }
    
    public string Back(int steps) {
        // Check bounds and adjust the current index
        currentIndex = Math.Max(0, currentIndex - steps);
        return history[currentIndex];
    }
    
    public string Forward(int steps) {
        // Check bounds and adjust the current index
        currentIndex = Math.Min(history.Count - 1, currentIndex + steps);
        return history[currentIndex];
    }
}
```

#### Complexity
- **Time Complexity:** 
  - `Visit`: O(n) in the worst case due to slicing the list.
  - `Back`/`Forward`: O(1).

- **Space Complexity:** O(n) where n is the number of URL visits stored.

### Approach 2: Efficient Bidirectional Linked List

#### Intuition
A doubly linked list is more suited to problems like these because it allows O(1) operations for moves in either direction and modifications at the current position (such as visiting a new page).

#### Steps
1. Implement a doubly linked list where each node contains a URL and pointers to the previous and next nodes.
2. On visiting a new page:
   - Drop all nodes beyond the current one since they become invalid.
   - Insert the new node and move the pointer to it.
3. On back/forward actions:
   - Adjust movement by simply pointing to the previous or next node as required, ensuring bound checks.

```csharp
public class Node {
    public string Url;
    public Node Prev, Next;

    public Node(string url) {
        Url = url;
        Prev = null;
        Next = null;
    }
}

public class BrowserHistory {
    private Node current;

    public BrowserHistory(string homepage) {
        current = new Node(homepage);
    }
    
    public void Visit(string url) {
        Node newNode = new Node(url);
        current.Next = newNode;
        newNode.Prev = current;
        current = newNode; // Move current to new node
    }
    
    public string Back(int steps) {
        while (current.Prev != null && steps > 0) {
            current = current.Prev;
            steps--;
        }
        return current.Url;
    }
    
    public string Forward(int steps) {
        while (current.Next != null && steps > 0) {
            current = current.Next;
            steps--;
        }
        return current.Url;
    }
}
```

#### Complexity
- **Time Complexity:** O(1) for all `Visit`, `Back`, and `Forward` operations as node referencing and pointer updates are done in constant time.
  
- **Space Complexity:** O(n) for storing URLs in nodes of the doubly linked list, where n is the total number of visits. 

In conclusion, the second approach offers optimal time performance for operations which makes it ideal for applications requiring large-scale history management.

