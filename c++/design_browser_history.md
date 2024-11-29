# 1472. [Design Browser History](https://leetcode.com/problems/design-browser-history/)

## Approach: Doubly Linked List

### Solution
cpp
```cpp
// Time Complexity:
//   - visit: O(1)
//   - back: O(steps)
//   - forward: O(steps)
// Space Complexity: O(n), where n is the number of visited pages
class BrowserHistory {
private:
    struct Node {
        std::string url;
        Node* prev;
        Node* next;

        Node(const std::string& url) : url(url), prev(nullptr), next(nullptr) {}
    };

    Node* current;

public:
    BrowserHistory(const std::string& homepage) {
        current = new Node(homepage);
    }

    void visit(const std::string& url) {
        Node* newNode = new Node(url);
        current->next = newNode;
        newNode->prev = current;
        current = newNode; // Move to the newly visited page
    }

    std::string back(int steps) {
        while (steps > 0 && current->prev != nullptr) {
            current = current->prev;
            steps--;
        }
        return current->url;
    }

    std::string forward(int steps) {
        while (steps > 0 && current->next != nullptr) {
            current = current->next;
            steps--;
        }
        return current->url;
    }
};

/**
 * Your BrowserHistory object will be instantiated and called as such:
 * BrowserHistory* obj = new BrowserHistory(homepage);
 * obj->visit(url);
 * std::string param_2 = obj->back(steps);
 * std::string param_3 = obj->forward(steps);
 */
```

