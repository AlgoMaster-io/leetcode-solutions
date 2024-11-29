# 1472. [Design Browser History](https://leetcode.com/problems/design-browser-history/)

## Approach: Doubly Linked List

### Solution
python
```python
# Time Complexity:
#   - visit: O(1)
#   - back: O(steps)
#   - forward: O(steps)
# Space Complexity: O(n), where n is the number of visited pages
class Node:
    def __init__(self, url: str):
        self.url = url
        self.prev = None
        self.next = None

class BrowserHistory:
    def __init__(self, homepage: str):
        self.current = Node(homepage)

    def visit(self, url: str) -> None:
        new_node = Node(url)
        self.current.next = new_node
        new_node.prev = self.current
        self.current = new_node  # Move to the newly visited page

    def back(self, steps: int) -> str:
        while steps > 0 and self.current.prev is not None:
            self.current = self.current.prev
            steps -= 1
        return self.current.url

    def forward(self, steps: int) -> str:
        while steps > 0 and self.current.next is not None:
            self.current = self.current.next
            steps -= 1
        return self.current.url

# Example usage:
# obj = BrowserHistory(homepage)
# obj.visit(url)
# param_2 = obj.back(steps)
# param_3 = obj.forward(steps)
```

