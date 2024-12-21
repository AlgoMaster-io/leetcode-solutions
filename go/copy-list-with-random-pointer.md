# [Copy List with Random Pointer](https://leetcode.com/problems/copy-list-with-random-pointer/)

## Solutions
- [Approach 1: Using a HashMap (Two-pass approach)](#approach-1-using-a-hashmap-two-pass-approach)
- [Approach 2: Optimized In-Place (Three-pass approach)](#approach-2-optimized-in-place-three-pass-approach)

---

## Approach 1: Using a HashMap (Two-pass approach)

### Intuition
The random pointer makes it more complex than a simple copy. By utilizing a hash map, we can store the mapping of original nodes to their corresponding new nodes. This approach involves two primary steps: first, create a new corresponding node for each original node and store it in the map. In the second pass, set the `next` and `random` pointers using the map.

### Steps:
1. **First Pass**: 
   - Iterate over the original list and clone each node.
   - Store the original node as key and the cloned node as the value in a hash map.

2. **Second Pass**:
   - Iterate again and set `next` and `random` pointers for each cloned node using entries from the hash map.

### Code
```go
type Node struct {
	Val    int
	Next   *Node
	Random *Node
}

func copyRandomList(head *Node) *Node {
	if head == nil {
		return nil
	}

	// Hashmap to store the mapping from original node to the cloned node
	nodeMap := make(map[*Node]*Node)

	// First pass to copy all nodes
	current := head
	for current != nil {
		nodeMap[current] = &Node{Val: current.Val}
		current = current.Next
	}

	// Second pass to assign next and random pointers
	current = head
	for current != nil {
		if current.Next != nil {
			nodeMap[current].Next = nodeMap[current.Next]
		}
		if current.Random != nil {
			nodeMap[current].Random = nodeMap[current.Random]
		}
		current = current.Next
	}

	return nodeMap[head]
}
```

### Complexity
- **Time Complexity**: O(n), where n is the number of nodes in the linked list since we iterate the list twice.
- **Space Complexity**: O(n), as we use a hash map to store the mapping of the original nodes to the copied nodes.

---

## Approach 2: Optimized In-Place (Three-pass approach)

### Intuition
Instead of using extra space for a hash map, modify the original list to interleave copied nodes. This will let us handle the `random` pointers in an efficient manner without additional space. After setting all pointers of the copied nodes, restore the original list. This approach saves space by leveraging the structure of the list.

### Steps:
1. **First Pass**: 
   - Create new cloned nodes and link each cloned node next to its original node, resulting in interleaved new nodes.

2. **Second Pass**:
   - Assign random pointers to all copied nodes through interleaving.

3. **Third Pass**:
   - Restore the original linked list and separate the copied list.

### Code
```go
func copyRandomList(head *Node) *Node {
	if head == nil {
		return nil
	}

	// Step 1: Interleave cloned nodes with original nodes
	current := head
	for current != nil {
		nextOriginal := current.Next
		clonedNode := &Node{Val: current.Val}
		current.Next = clonedNode
		clonedNode.Next = nextOriginal
		current = nextOriginal
	}

	// Step 2: Assign random pointers for the cloned nodes
	current = head
	for current != nil {
		if current.Random != nil {
			current.Next.Random = current.Random.Next // Link cloned random
		}
		current = current.Next.Next // Move to the next original node
	}

	// Step 3: Restore the original list and extract the copied list
	current = head
	copiedHead := head.Next
	for current != nil {
		clonedNode := current.Next
		current.Next = clonedNode.Next // restore original list's next node
		current = current.Next
		if current != nil {
			clonedNode.Next = current.Next // link copied list's next node
		}
	}

	return copiedHead
}
```

### Complexity
- **Time Complexity**: O(n), where n is the number of nodes in the linked list since we perform a constant time operation for each node three times.
- **Space Complexity**: O(1), as we don't use any extra space apart from the cloned nodes themselves.

