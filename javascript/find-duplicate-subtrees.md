[Leetcode 652: Find Duplicate Subtrees](https://leetcode.com/problems/find-duplicate-subtrees/)

## Approaches
1. [Brute Force - Compare All Subtrees](#approach-1)
2. [Optimal - Serialize Subtree + HashMap](#approach-2)

---

### Approach 1: Brute Force - Compare All Subtrees

**Intuition:**

The naive approach to solving this problem is to compare all possible subtrees of the binary tree with each other and identify duplicates. This involves traversing each subtree and checking if it matches with any other previously traversed subtree. Though straightforward, this method lacks efficiency due to its excessive comparisons.

**Steps:**

1. Traverse each node in the tree and treat it as the root of a subtree.
2. For each subtree rooted at a particular node, compare it with all other subtrees to check for duplicacy.
3. If two subtrees match, store them in a set to ensure unique duplicates.
4. Finally, convert the set to a list and return.

**Code:**

```javascript
function findDuplicateSubtrees(root) {
    const subtrees = [];

    // Helper function to find if two trees are identical
    function isIdentical(root1, root2) {
        if (!root1 && !root2) return true;
        if (!root1 || !root2) return false;
        return (root1.val === root2.val) &&
               isIdentical(root1.left, root2.left) &&
               isIdentical(root1.right, root2.right);
    }

    // Main function to find duplicate subtrees
    function findDuplicates(node) {
        if (!node) return;
        
        // Compare this node's subtree with all other nodes' subtrees
        for (let i = 0; i < subtrees.length; i++) {
            if (isIdentical(node, subtrees[i])) {
                return; // Already captured as a duplicate
            }
        }
        
        // Add the current subtree root if it's a new duplicate
        subtrees.push(node);
        
        // Recurse on left and right children
        findDuplicates(node.left);
        findDuplicates(node.right);
    }
    
    // Initial call
    findDuplicates(root);
    return subtrees;
}
```

**Complexity:**

- Time Complexity: O(n^2 * m), where n is the number of nodes and m is the average number of nodes within a subtree.
- Space Complexity: O(n) for storing subtree roots in an array.

---

### Approach 2: Optimal - Serialize Subtree + HashMap

**Intuition:**

A more efficient way utilizes serialization of subtrees combined with a hash map to store frequencies of each serialized subtree. By serializing, we can uniquely identify a subtree structure and check for its duplicates efficiently.

**Steps:**

1. Serialize each subtree using a post-order traversal (left, right, node).
2. Use a HashMap to map each serialized subtree string to its frequency.
3. For each subtree string seen more than once, add the root node of one occurrence to the result list.
4. Return the list of root nodes of duplicate subtrees.

**Code:**

```javascript
function findDuplicateSubtrees(root) {
    const map = new Map();
    const result = [];

    // Helper function to serialize subtree
    function serialize(node) {
        if (!node) return "#"; // Use '#' for null to ensure uniqueness
        
        const left = serialize(node.left);
        const right = serialize(node.right);
        const serialized = `${left},${right},${node.val}`;
        
        if (map.has(serialized)) {
            map.set(serialized, map.get(serialized) + 1);
            if (map.get(serialized) === 2) { // Only add duplicate the first time we encounter it as a duplicate
                result.push(node);
            }
        } else {
            map.set(serialized, 1);
        }
        
        return serialized;
    }
    
    // Start serialization and find duplicates
    serialize(root);
    
    return result;
}
```

**Complexity:**

- Time Complexity: O(n), where n is the number of nodes since each node is visited once.
- Space Complexity: O(n), for storing serialized subtree strings and duplicates in the result list. 

By utilizing serialization and a hashmap to keep track of duplicate subtrees efficiently, this approach optimizes both time and space over the naive brute force solution.

