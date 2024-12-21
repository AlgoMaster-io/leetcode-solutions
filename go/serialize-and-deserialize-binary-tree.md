# [LeetCode 297: Serialize and Deserialize Binary Tree](https://leetcode.com/problems/serialize-and-deserialize-binary-tree/)

## Approaches:
1. [Simple BFS (Breadth-First Search) Approach](#simple-bfs-breadth-first-search-approach)
2. [Optimal DFS (Depth-First Search) Approach](#optimal-dfs-depth-first-search-approach)

### Simple BFS (Breadth-First Search) Approach

**Intuition**:
- The goal is to convert a binary tree into a string (serialization) and back into the tree (deserialization).
- We can use level-order traversal (BFS) to serialize the tree. We can represent null nodes with a special character (e.g., "null").
- To deserialize, we can utilize a queue to reconstruct the tree from the serialized string.

**Steps**:
1. **Serialization**: 
   - Use a queue to traverse the tree by levels.
   - Enqueue left and right children of each node, using "null" for absent children.
   - Append the node value (or "null") to the result string.
   
2. **Deserialization**:
   - Split the serialized string by commas to get an array of node values.
   - Use a queue to help reconstruct the tree. Start with the root node.
   - For each node, use the next two values to set its left and right children.
   
3. **Complexity**:
   - Time complexity for both serialization and deserialization is O(n), where n is the number of nodes.
   - Space complexity is O(n) due to the use of queues and output storage.

```go
package main

import (
	"strings"
	"strconv"
)

// TreeNode definition
type TreeNode struct {
    Val   int
    Left  *TreeNode
    Right *TreeNode
}

// Codec for serialization and deserialization
type Codec struct{}

func Constructor() Codec {
    return Codec{}
}

// Serializes a tree to a single string using BFS
func (this *Codec) serialize(root *TreeNode) string {
    if root == nil {
        return ""
    }
    var result []string
    queue := []*TreeNode{root}
    
    for len(queue) > 0 {
        node := queue[0]
        queue = queue[1:]
        
        if node != nil {
            result = append(result, strconv.Itoa(node.Val))
            queue = append(queue, node.Left, node.Right)
        } else {
            result = append(result, "null")
        }
    }
    return strings.Join(result, ",")
}

// Deserializes your encoded data to tree using BFS
func (this *Codec) deserialize(data string) *TreeNode {
    if data == "" {
        return nil
    }
    
    values := strings.Split(data, ",")
    if len(values) == 0 {
        return nil
    }
    
    rootVal, _ := strconv.Atoi(values[0])
    root := &TreeNode{Val: rootVal}
    queue := []*TreeNode{root}
    index := 1
    
    for len(queue) > 0 {
        node := queue[0]
        queue = queue[1:]
        
        if values[index] != "null" {
            leftVal, _ := strconv.Atoi(values[index])
            node.Left = &TreeNode{Val: leftVal}
            queue = append(queue, node.Left)
        }
        index++
        
        if values[index] != "null" {
            rightVal, _ := strconv.Atoi(values[index])
            node.Right = &TreeNode{Val: rightVal}
            queue = append(queue, node.Right)
        }
        index++
    }
    
    return root
}

// Example usage
// codec := Constructor()
// tree := codec.deserialize("1,2,3,null,null,4,5")
// serialized := codec.serialize(tree)
```

### Optimal DFS (Depth-First Search) Approach

**Intuition**:
- DFS can be used to represent the tree structure efficiently.
- We serialize the tree using preorder traversal, which records a node before its children. Including null nodes in this traversal keeps the tree structure intact.
- Deserialization involves reconstructing the tree using the preorder sequence.

**Steps**:
1. **Serialization**:
   - Perform a recursive preorder traversal.
   - Append node values and use a marker for null nodes.
   
2. **Deserialization**:
   - Recursively rebuild the tree using the preorder sequence.
   
3. **Complexity**:
   - Time complexity is O(n) for both serialization and deserialization.
   - Space complexity is O(n) due to recursive stack space in DFS.

```go
package main

import (
	"strings"
	"strconv"
)

// TreeNode definition
type TreeNode struct {
    Val   int
    Left  *TreeNode
    Right *TreeNode
}

// Codec for serialization and deserialization
type Codec struct{}

func Constructor() Codec {
    return Codec{}
}

// Serializes a tree to a single string using DFS
func (this *Codec) serialize(root *TreeNode) string {
    var sb strings.Builder
    serializeHelper(root, &sb)
    return sb.String()
}

func serializeHelper(node *TreeNode, sb *strings.Builder) {
    if node == nil {
        sb.WriteString("null,")
        return
    }
    sb.WriteString(strconv.Itoa(node.Val) + ",")
    serializeHelper(node.Left, sb)
    serializeHelper(node.Right, sb)
}

// Deserializes your encoded data to tree using DFS
func (this *Codec) deserialize(data string) *TreeNode {
    values := strings.Split(data, ",")
    index := 0
    return deserializeHelper(&values, &index)
}

func deserializeHelper(values *[]string, index *int) *TreeNode {
    if *index >= len(*values) || (*values)[*index] == "null" {
        *index++
        return nil
    }
    
    val, _ := strconv.Atoi((*values)[*index])
    *index++
    node := &TreeNode{Val: val}
    node.Left = deserializeHelper(values, index)
    node.Right = deserializeHelper(values, index)
    
    return node
}

// Example usage
// codec := Constructor()
// tree := codec.deserialize("1,2,null,null,3,4,null,null,5,null,null,")
// serialized := codec.serialize(tree)
```

In both approaches, the choice of BFS or DFS will depend on one's preference or the specific requirements of an utilization scenario. Both offer an efficient O(n) time complexity solution, handling binary trees of significant size gracefully.

