# [Leetcode 652: Find Duplicate Subtrees](https://leetcode.com/problems/find-duplicate-subtrees/)

## Approaches
1. [Serialize Subtrees with Pre-order Traversal](#serialize-subtrees-with-pre-order-traversal)
2. [Optimal Approach Using Hashmap and Serialization](#optimal-approach-using-hashmap-and-serialization)

### Serialize Subtrees with Pre-order Traversal

#### Intuition:
The idea is to serialize the subtrees and record their frequencies. For each subtree rooted at a node, we can represent it as a string in pre-order traversal format. By storing these serialized strings in a hashmap with their frequencies, we can identify duplicates since their frequencies will be greater than 1.

#### Approach:
1. Traverse the tree in a post-order manner.
2. Serialize each subtree by returning a string representation, using sentinel values for null nodes.
3. For each serialized subtree, add it to a map to track its frequency.
4. Collect all subtrees with a frequency greater than 1.

#### Time Complexity:
- O(n^2) in the worst case, where n is the number of nodes. Each serialization may take O(n).

#### Space Complexity:
- O(n), due to the hashmap storing the subtree strings and recursion stack in case of unbalanced tree.

```go
// Definition for a binary tree node.
type TreeNode struct {
    Val   int
    Left  *TreeNode
    Right *TreeNode
}

func findDuplicateSubtrees(root *TreeNode) []*TreeNode {
    subtreeMap := make(map[string]int)
    var result []*TreeNode

    var serializeAndFind func(node *TreeNode) string
    serializeAndFind = func(node *TreeNode) string {
        if node == nil {
            return "#"
        }

        // Create a serialization of the tree
        left := serializeAndFind(node.Left)
        right := serializeAndFind(node.Right)
        subtree := fmt.Sprintf("%d,%s,%s", node.Val, left, right)

        // Count the number of times this serialization appears
        count := subtreeMap[subtree]
        if count == 1 {
            result = append(result, node) // Only add to result on first duplicate
        }
        subtreeMap[subtree] = count + 1

        return subtree
    }

    serializeAndFind(root)
    return result
}
```

### Optimal Approach Using Hashmap and Serialization

#### Intuition:
To optimize, we avoid recomputing string hashes by assigning unique identifiers to each unique serialized subtree. Instead of maintaining complete subtree strings, we hash the structure and content efficiently. With this improved frequency checking, we reduce computational overhead.

#### Approach:
1. Use a map to assign unique identifiers to each subtree serialization.
2. Serialize each subtree similar to the previous approach but convert it to an identifier.
3. Retain subtree roots as duplicates only when their serialized forms appear more than once.

#### Time Complexity:
- O(n), since serialization, look-ups and assignments in hashmap are constant time on average.

#### Space Complexity:
- O(n), for storing the subtree hashes and recursion stack.

```go
type TreeNode struct {
    Val   int
    Left  *TreeNode
    Right *TreeNode
}

func findDuplicateSubtrees(root *TreeNode) []*TreeNode {
    idMap := make(map[string]int) // Map serialization to unique id
    countMap := make(map[int]int) // Count occurrences of each id
    var result []*TreeNode
    var idCounter int

    var serializeAndFind func(node *TreeNode) int
    serializeAndFind = func(node *TreeNode) int {
        if node == nil {
            return 0 // Use 0 as a sentinel id for nil nodes
        }

        // Serialize current subtree
        leftId := serializeAndFind(node.Left)
        rightId := serializeAndFind(node.Right)
        subtree := fmt.Sprintf("%d,%d,%d", node.Val, leftId, rightId)

        id, exists := idMap[subtree]
        if !exists {
            idCounter++
            id = idCounter
            idMap[subtree] = id
        }

        countMap[id]++
        if countMap[id] == 2 { // Add to result only on first duplicate
            result = append(result, node)
        }

        return id
    }

    serializeAndFind(root)
    return result
}
```

In this optimal solution, instead of storing potentially long serialized strings, we efficiently track subtree structures using identifiers, reducing the overall time complexity and improving scalability on larger trees.

