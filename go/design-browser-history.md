# [Leetcode 1472: Design Browser History](https://leetcode.com/problems/design-browser-history)

## Approaches
1. [Using a Doubly Linked List](#using-a-doubly-linked-list)
2. [Using Two Stacks](#using-two-stacks)

---

### 1. Using a Doubly Linked List

**Intuition**

The Browser History problem can be likened to navigating back and forth through previously visited pages. A doubly linked list is a suitable data structure because it efficiently supports forward and backward traversal. Each node in the list represents a page, and the `next` and `prev` pointers help us move through the history.

**Detailed Explanation**

- Initialize the linked list with a head node representing the initial home page.
- Whenever a new URL is visited using the `visit` method, we discard all pages ahead of the current page and append this new page as the next node.
- The `back` method navigates backward by the specified steps or until the head of the list is reached.
- The `forward` method navigates forward by the specified steps or until the latest node is reached.

**Code**

```go
type Node struct {
    url  string
    prev *Node
    next *Node
}

type BrowserHistory struct {
    current *Node
}

func Constructor(homepage string) BrowserHistory {
    node := &Node{url: homepage}
    return BrowserHistory{current: node}
}

func (this *BrowserHistory) Visit(url string) {
    // Create a new node for the visited page
    newNode := &Node{url: url}
    // Break the forward history
    this.current.next = nil
    // Attach it to the current node's next, and set its previous
    newNode.prev = this.current
    this.current.next = newNode
    // Move current to this new node
    this.current = newNode
}

func (this *BrowserHistory) Back(steps int) string {
    // Move backwards in the linked list up to 'steps' or until reaching the head
    for steps > 0 && this.current.prev != nil {
        this.current = this.current.prev
        steps--
    }
    return this.current.url
}

func (this *BrowserHistory) Forward(steps int) string {
    // Move forwards in the linked list up to 'steps' or until reaching the last node
    for steps > 0 && this.current.next != nil {
        this.current = this.current.next
        steps--
    }
    return this.current.url
}
```

**Time Complexity**

- `Visit`: O(1) — We're just updating pointers.
- `Back` & `Forward`: O(min(n, steps)) — Where n is the number of nodes in the linked list. In the worst-case, we navigate through all nodes.

**Space Complexity**

- O(n) — For storing each visited URL as a node in the list.

---

### 2. Using Two Stacks

**Intuition**

Another way to resolve the history navigation problem is using two stacks: one for the backward history and one for the forward history. In this way, we manage separate histories for pages navigated backward and forward.

**Detailed Explanation**

- The `backward` stack keeps track of pages that have been navigated back from.
- The `forward` stack keeps track of pages that have been visited using `back` since they may be revisited using `forward`.
- Upon `visit`, clear the `forward` stack since it represents a new history path.
- When calling `back`, pop from the `backward` stack and push onto the `forward` stack.
- Conversely, when calling `forward`, pop from the `forward` stack and push onto the `backward` stack.

**Code**

```go
type BrowserHistory struct {
    current  string
    backward []string
    forward  []string
}

func Constructor(homepage string) BrowserHistory {
    return BrowserHistory{current: homepage}
}

func (this *BrowserHistory) Visit(url string) {
    // Push current page to backward stack
    this.backward = append(this.backward, this.current)
    // Clear the forward stack upon visiting a new page
    this.forward = nil
    // Set the current page to the new url
    this.current = url
}

func (this *BrowserHistory) Back(steps int) string {
    // Move from current to backward stack while steps remain and there is a backward history
    for steps > 0 && len(this.backward) > 0 {
        steps--
        // Push current to the forward stack
        this.forward = append(this.forward, this.current)
        // Pop from backward stack to the current page
        this.current = this.backward[len(this.backward)-1]
        this.backward = this.backward[:len(this.backward)-1]
    }
    return this.current
}

func (this *BrowserHistory) Forward(steps int) string {
    // Move from current to forward stack while steps remain and there is a forward history
    for steps > 0 && len(this.forward) > 0 {
        steps--
        // Push current to the backward stack
        this.backward = append(this.backward, this.current)
        // Pop from forward stack to the current page
        this.current = this.forward[len(this.forward)-1]
        this.forward = this.forward[:len(this.forward)-1]
    }
    return this.current
}
```

**Time Complexity**

- `Visit`: O(1) — Push to stack.
- `Back` & `Forward`: O(steps) — Limited by the number of steps and the size of stacks.

**Space Complexity**

- O(n) — For storing each visited URL in stacks, with n being the number of visited pages.

Both approaches have their utilities, with the stack-based one often being more intuitive due to simplistic push-pop mechanics. On the other hand, the linked-list approach can provide clearer bidirectional navigation logic. Both methods achieve the needed history navigation effectively.

