# [Leetcode 1472: Design Browser History](https://leetcode.com/problems/design-browser-history/)

## Approaches
1. [Basic Approach - Using Two Stacks](#basic-approach---using-two-stacks)
2. [Optimized Approach - Using Doubly Linked List](#optimized-approach---using-doubly-linked-list)

---

## Basic Approach - Using Two Stacks

### Intuition
In this basic approach, two stacks are used to simulate the forward and backward navigation of a browser. One stack will hold the history of visited URLs, while the other will keep track of the forward history. The main idea is to use these structures to manage navigation efficiently by popping and pushing URLs as we move back and forth.

### Approach
1. Initialize two stacks: `backward_history` and `forward_history`.
2. When visiting a new URL, push the current URL (if any) onto the backward history stack, clear the forward history (since forward history is invalid after a new visit), and then push the new URL as the current page.
3. For the backward operation, if the backward stack is not empty, pop the top URL from it, push the current page onto the forward stack, and set the popped URL as the current page.
4. For the forward operation, if the forward stack is not empty, pop the top URL, push the current page onto the backward stack, and set the popped URL as the current page.

### Time Complexity
- `visit()`: O(1)
- `back()`: O(min(steps, size of backward_history))
- `forward()`: O(min(steps, size of forward_history))

### Space Complexity
O(N), where N is the number of total URLs visited since we have a stack holding each visited URL once.

### Code
```cpp
#include <iostream>
#include <stack>
#include <string>

class BrowserHistory {
private:
    std::stack<std::string> backward_history, forward_history;
    std::string current_page;

public:
    BrowserHistory(std::string homepage) {
        current_page = homepage;
    }
    
    void visit(std::string url) {
        // Store the current page to the backward history
        backward_history.push(current_page);
        // Set the new URL as the current page
        current_page = url;
        // Clear forward history as it's invalid after visiting a new page
        while (!forward_history.empty()) {
            forward_history.pop();
        }
    }
    
    std::string back(int steps) {
        // Move back 'steps' times or until there is no page left in backward history
        while (steps > 0 && !backward_history.empty()) {
            forward_history.push(current_page);
            current_page = backward_history.top();
            backward_history.pop();
            steps--;
        }
        return current_page;
    }
    
    std::string forward(int steps) {
        // Go forward 'steps' times or until there is no page left in forward history
        while (steps > 0 && !forward_history.empty()) {
            backward_history.push(current_page);
            current_page = forward_history.top();
            forward_history.pop();
            steps--;
        }
        return current_page;
    }
};
```

---

## Optimized Approach - Using Doubly Linked List

### Intuition
The doubly linked list is a natural fit for this problem because it allows bidirectional traversal, mimicking the backward and forward navigation of a browser. Each node in the list represents a page, with pointers to the previous and next pages.

### Approach
1. Create a doubly linked list where each node represents a URL. Initialize it with the homepage.
2. On the visit operation, create a new node for the URL, link it as the `next` of the current node, and update current. Remove any nodes that existed after the current node since they become obsolete.
3. For the backward operation, move the current node pointer up to `k` times to its `prev` node.
4. For the forward operation, move the current node pointer up to `k` times to its `next` node.

### Time Complexity
- `visit()`: O(1)
- `back()`: O(min(steps, number of back available))
- `forward()`: O(min(steps, number of forward available))

### Space Complexity
O(N), where N is the number of unique URLs visited, since each URL is stored exactly once in the list.

### Code
```cpp
#include <iostream>
#include <string>

class BrowserHistory {
private:
    struct Node {
        std::string url;
        Node* prev;
        Node* next;
        Node(std::string u) : url(u), prev(nullptr), next(nullptr) {}
    };
    
    Node* current;

public:
    BrowserHistory(std::string homepage) {
        // Initialize with the homepage
        current = new Node(homepage);
    }
    
    void visit(std::string url) {
        // Create a new node for the new URL
        Node* newNode = new Node(url);
        // Attach it to the current node
        current->next = newNode;
        newNode->prev = current;
        // Move the current pointer to the new URL
        current = newNode;
    }
    
    std::string back(int steps) {
        // Move the current pointer backward up to steps available
        while (steps > 0 && current->prev != nullptr) {
            current = current->prev;
            steps--;
        }
        return current->url;
    }
    
    std::string forward(int steps) {
        // Move the current pointer forward up to steps available
        while (steps > 0 && current->next != nullptr) {
            current = current->next;
            steps--;
        }
        return current->url;
    }
};
```

This approach leverages the bidirectional nature of doubly linked lists to efficiently handle navigation between pages and is thus more memory efficient in terms of space used for temporary forward history.

