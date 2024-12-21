# [Leetcode 729: My Calendar I](https://leetcode.com/problems/my-calendar-i/)

## Approaches:
1. [Brute Force Approach](#brute-force-approach)
2. [Binary Search Tree Approach](#binary-search-tree-approach)

---

### Brute Force Approach

In the brute force approach, we can maintain a list of already booked slots and for every new booking request, we will iterate through this list to check if the new booking overlaps with any existing booking.

#### Intuition:
- We maintain a simple list of pairs where each pair represents a bookend time.
- For each new booking request, we will check this entire list to prevent overlapping.
- If the new booking does not overlap with any of the existing ones, we add it to our list.

#### Implementation:

```go
type MyCalendar struct {
    bookings [][2]int
}

func Constructor() MyCalendar {
    return MyCalendar{
        bookings: make([][2]int, 0),
    }
}

func (this *MyCalendar) Book(start int, end int) bool {
    // Iterate over each booked slot
    for _, booking := range this.bookings {
        // If any existing booking overlaps with the new one, reject it
        if start < booking[1] && end > booking[0] {
            return false
        }
    }
    // If no overlapping is found, add the booking
    this.bookings = append(this.bookings, [2]int{start, end})
    return true
}
```

#### Time and Space Complexity:
- **Time Complexity:** O(n), where n is the number of bookings made so far, because for each booking, we check against all previous bookings.
- **Space Complexity:** O(n), for storing the bookings.

---

### Binary Search Tree Approach

Here, instead of storing the bookings in a simple list, we maintain them in a Binary Search Tree (BST) structure. Each node in the tree represents a booking.

#### Intuition:
- The key idea is to leverage the BST properties to efficiently search for overlap.
- We maintain a balanced BST where each node contains the start and end of a booking.
- When a new booking request comes in, we traverse the tree to find the appropriate location.
- During the traversal, we also check for overlap.

#### Implementation:

```go
type TreeNode struct {
    start, end int
    left, right *TreeNode
}

type MyCalendar struct {
    root *TreeNode
}

func Constructor() MyCalendar {
    return MyCalendar{
        root: nil,
    }
}

func (this *MyCalendar) Book(start int, end int) bool {
    if this.root == nil {
        this.root = &TreeNode{start, end, nil, nil}
        return true
    }
    return this.insert(this.root, start, end)
}

func (this *MyCalendar) insert(node *TreeNode, start, end int) bool {
    // If the new interval is to the right and non-overlapping, move right
    if start >= node.end {
        if node.right == nil {
            node.right = &TreeNode{start, end, nil, nil}
            return true
        }
        return this.insert(node.right, start, end)
    } 
    // If the new interval is to the left and non-overlapping, move left
    if end <= node.start {
        if node.left == nil {
            node.left = &TreeNode{start, end, nil, nil}
            return true
        }
        return this.insert(node.left, start, end)
    }
    // Overlapping case
    return false
}
```

#### Time and Space Complexity:
- **Time Complexity:** O(log n) on average if the tree is balanced, but O(n) in the worst case (when the tree becomes skewed either left or right).
- **Space Complexity:** O(n), for storing the nodes in the tree.

Overall, the BST approach provides a more efficient solution in terms of time complexity, especially when the bookings are numerous.

