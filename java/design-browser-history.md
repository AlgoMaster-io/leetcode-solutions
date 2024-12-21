# [Leetcode 1472: Design Browser History](https://leetcode.com/problems/design-browser-history/)

## Approaches
- [Approach 1: Using a List to Store History](#approach-1-using-a-list-to-store-history)
- [Approach 2: Using Two Stacks](#approach-2-using-two-stacks)

### Approach 1: Using a List to Store History

**Intuition:**
The most straightforward approach to manage browsing history is by using a list. Each time we visit a new URL, we can append it to the list from the current position, effectively simulating moving forward or backward in browser history.

**Execution:**
- Use an ArrayList to store the web pages.
- Maintain an integer `current` to track the index of the current web page.
- On visiting a new page, all forward history (if any) is removed, and the new URL is appended.

**Details:**
1. **visit(String url):** Add the url to the history at the current position and remove any forward history. Update the current position.
2. **back(int steps):** Move the current position back by `steps` or until reaching the first page.
3. **forward(int steps):** Move the current position forward by `steps` or until reaching the last page.

```java
class BrowserHistory {
    private List<String> history;
    private int current;

    public BrowserHistory(String homepage) {
        history = new ArrayList<>();
        history.add(homepage);
        current = 0;
    }
    
    public void visit(String url) {
        // Remove forward history
        while (history.size() > current + 1) {
            history.remove(history.size() - 1);
        }
        // Add new page and update current index
        history.add(url);
        current++;
    }
    
    public String back(int steps) {
        // Move current index back, check bounds
        current = Math.max(0, current - steps);
        return history.get(current);
    }
    
    public String forward(int steps) {
        // Move current index forward, check bounds
        current = Math.min(history.size() - 1, current + steps);
        return history.get(current);
    }
}
```

**Time Complexity:** 
- `visit()`: O(N) in the worst case (removing forward history), where N is the size of the current list.
- `back()` and `forward()`: O(1).

**Space Complexity:** O(N), where N is the number of URLs stored.

### Approach 2: Using Two Stacks

**Intuition:**
To optimize the operations for forward and backward navigation, two stacks can be used:
- One stack to hold the history we can go back to.
- Another stack to hold the history we can move forward to.

**Execution:**
- Use a current variable to track the active page.
- On `visit`, clear the forward stack and push the current page to the backward stack before visiting the new page.
- For `back`, push the current page to the forward stack and pop from the backward stack.
- For `forward`, pop from the forward stack and push to the backward stack.

**Details:**
1. **visit(String url):** Push current page to the back stack, clear forward stack, and set visited page as current.
2. **back(int steps):** Move as many steps back from the back stack, adjusting the current page.
3. **forward(int steps):** Move as many steps forward from the forward stack, adjusting the current page.

```java
class BrowserHistory {
    private Stack<String> backStack;
    private Stack<String> forwardStack;
    private String current;

    public BrowserHistory(String homepage) {
        backStack = new Stack<>();
        forwardStack = new Stack<>();
        current = homepage;
    }
    
    public void visit(String url) {
        // Push current to backStack and set new visit
        backStack.push(current);
        current = url;
        forwardStack.clear(); // Clear forwardStack since we are branching a new path
    }
    
    public String back(int steps) {
        // Move backward steps, ensuring not move past initial page
        while (steps > 0 && !backStack.isEmpty()) {
            forwardStack.push(current);
            current = backStack.pop();
            steps--;
        }
        return current;
    }
    
    public String forward(int steps) {
        // Move forward steps, ensuring not move past recently visited page
        while (steps > 0 && !forwardStack.isEmpty()) {
            backStack.push(current);
            current = forwardStack.pop();
            steps--;
        }
        return current;
    }
}
```

**Time Complexity:** 
- `visit()`: O(1). Adding to stack and clearing another.
- `back()` and `forward()`: O(steps). Maximum O(N) where N is the number of URLs.

**Space Complexity:** O(N), where N is the number of URLs stored.

