## [Leetcode 652: Find Duplicate Subtrees](https://leetcode.com/problems/find-duplicate-subtrees/)

### Solutions
1. [Naive Approach: Brute Force Comparison](#naive-approach-brute-force-comparison)
2. [Optimized Hashmap Solution Using Serialization](#optimized-hashmap-solution-using-serialization)

---

### Naive Approach: Brute Force Comparison

#### Intuition
The naive approach to finding duplicate subtrees would be to compare each subtree with every other subtree. To achieve this, for each node, you could traverse the tree, collect the subtree, and check for duplicates. However, this approach is inefficient for large trees due to repetitive checks and potential computational silos.

#### Time Complexity
- **Time:** O(n^2), where n is the number of nodes in the tree. For each node, comparing subtrees may cost up to O(n).
- **Space:** O(h + n*s), where h is the height of the tree and s is the size of the subtree structure used for each node.

> This approach isn't scalable due to its time inefficiency.

#### Typescript Code
```typescript
// TreeNode definition
class TreeNode {
    val: number;
    left: TreeNode | null;
    right: TreeNode | null;
    constructor(val?: number, left?: TreeNode | null, right?: TreeNode | null) {
        this.val = (val===undefined ? 0 : val);
        this.left = (left===undefined ? null : left);
        this.right = (right===undefined ? null : right);
    }
}

// Function to find duplicate subtrees using the naive approach
function findDuplicateSubtrees(root: TreeNode | null): TreeNode[] {
    const traverseSubtree = (node: TreeNode | null): string => {
        if (!node) return "#";
        const left = traverseSubtree(node.left);
        const right = traverseSubtree(node.right);
        return `(${left}${node.val}${right})`;
    };

    const subtrees: Map<string, number> = new Map();
    const duplicates: Set<TreeNode> = new Set();
    
    const dfs = (node: TreeNode | null) => {
        if (!node) return;
        
        const subtreeStr = traverseSubtree(node);
        if (subtrees.has(subtreeStr) && subtrees.get(subtreeStr) === 1) {
            duplicates.add(node);
        }

        subtrees.set(subtreeStr, (subtrees.get(subtreeStr) || 0) + 1);
        
        dfs(node.left);
        dfs(node.right);
    };

    dfs(root);
    return Array.from(duplicates);
}
```

---

### Optimized Hashmap Solution Using Serialization

#### Intuition
A more efficient solution leverages serialization of subtrees. By representing each subtree structure uniquely as a string (serialization), we can use a hashmap to track the frequency of each subtree serialization. When a serialization pattern is encountered more than once, it signifies a duplicate subtree.

#### Steps
1. Serialize each subtree.
2. Use a hashmap to count the frequency of each serialized subtree.
3. If any serialization has a count greater than 1, it indicates a duplicate subtree, and we can add the root of that subtree to our result.

#### Time Complexity
- **Time:** O(n), because each node and subtree are processed once.
- **Space:** O(n), for the hashmap used to store the serialized subtrees and their counts.

#### Typescript Code
```typescript
// TreeNode definition is already given above

// Function to find duplicate subtrees using optimized hashmap and serialization approach
function findDuplicateSubtreesOptimized(root: TreeNode | null): TreeNode[] {
    const subtreeCount: Map<string, number> = new Map();
    const duplicates: TreeNode[] = [];
    
    const serialize = (node: TreeNode | null): string => {
        if (!node) return "#"; // Base case for null nodes
        
        const left = serialize(node.left);
        const right = serialize(node.right);
        
        const subtreeStr = `${node.val},${left},${right}`;
        
        const count = subtreeCount.get(subtreeStr) || 0;
        if (count === 1) {
            // Duplicate subtree found, add node to result
            duplicates.push(node);
        }
        subtreeCount.set(subtreeStr, count + 1);
        
        return subtreeStr;
    };
    
    serialize(root);
    return duplicates;
}
```

This optimized solution is much more efficient and suitable for handling larger trees due to its linear time complexity.

