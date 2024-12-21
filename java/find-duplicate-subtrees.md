## [Leetcode 652: Find Duplicate Subtrees](https://leetcode.com/problems/find-duplicate-subtrees/)

### Navigation:
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Serialization with HashMap](#approach-2-serialization-with-hashmap)
- [Approach 3: Optimized Serialization with Unique ID](#approach-3-optimized-serialization-with-unique-id)

### Approach 1: Brute Force

**Intuition:**

The brute force approach involves comparing each subtree with every other subtree. We can implement this by conducting a traversal of the tree and checking each subtree for duplicates. However, this might replicate work since the same structure could be recomputed multiple times.

**Steps:**
1. Traverse each node in the tree.
2. Extract the subtree starting from that node.
3. Compare the extracted subtree with all other subtrees in the tree to check for duplicates.
4. Store and return the duplicate subtrees.

This approach is inefficient because of the repeated subtree comparisons.

**Code:**

```java
// Brute force is not implemented due to its inefficiency.
```

**Complexity Analysis:**
- **Time Complexity:** `O(n^2*m)`, where `n` is the number of nodes and `m` is the average number of nodes in each subtree.
- **Space Complexity:** `O(n*m)`, for storing subtrees and comparisons.

### Approach 2: Serialization with HashMap

**Intuition:**

Instead of comparing subtrees directly, we can serialize each subtree into a string and use a hashmap to count the frequency of each serialized subtree. This approach allows us to efficiently check if we've seen the same subtree structure before.

**Steps:**
1. Serialize each subtree rooted at each node into a string.
2. Use a HashMap to keep track of the frequency of each serialized subtree.
3. If the frequency of a serialized subtree is greater than `1`, it indicates a duplicate.
4. Keep track of the first instance of such duplicates.

**Code:**

```java
class Solution {
    Map<String, Integer> map = new HashMap<>();
    List<TreeNode> result = new ArrayList<>();

    public List<TreeNode> findDuplicateSubtrees(TreeNode root) {
        serialize(root);
        return result;
    }

    private String serialize(TreeNode root) {
        if (root == null) return "#";
        
        // Serialize subtree
        String serialized = root.val + "," + serialize(root.left) + "," + serialize(root.right);
        
        // Count the serialized subtree in map
        map.put(serialized, map.getOrDefault(serialized, 0) + 1);
        
        // If it's a duplicate, add to result only once
        if (map.get(serialized) == 2) result.add(root);
        
        return serialized;
    }
}
```

**Complexity Analysis:**
- **Time Complexity:** `O(n)`, because we traverse each node once and serialization is `O(1)` for each node.
- **Space Complexity:** `O(n)`, for storing serialized subtrees in the hashmap.

### Approach 3: Optimized Serialization with Unique ID

**Intuition:**

In the optimized approach, instead of using strings to serialize each subtree, we assign a unique ID to each distinct subtree structure. This reduces the space complexity as we avoid storing large strings.

**Steps:**
1. Use a map to assign a unique ID to each distinct subtree structure.
2. Maintain a second map to count the occurrence of these unique IDs.
3. If a subtree's unique ID occurs more than once, it indicates a duplicate subtree.

**Code:**

```java
class Solution {
    int id = 1;
    Map<String, Integer> trees = new HashMap<>();
    Map<Integer, Integer> count = new HashMap<>();
    List<TreeNode> result = new ArrayList<>();

    public List<TreeNode> findDuplicateSubtrees(TreeNode root) {
        lookup(root);
        return result;
    }

    private int lookup(TreeNode root) {
        if (root == null) return 0;
        
        // Serialize subtree
        String serial = root.val + "," + lookup(root.left) + "," + lookup(root.right);
        
        // Assign ID to serialized subtree
        int treeId = trees.computeIfAbsent(serial, x -> id++);
        
        // Count the occurrence of the tree ID
        count.put(treeId, count.getOrDefault(treeId, 0) + 1);
        
        // If it's the second occurrence of the same subtree, record it
        if (count.get(treeId) == 2) result.add(root);
        
        return treeId;
    }
}
```

**Complexity Analysis:**
- **Time Complexity:** `O(n)`, traversing each node once and using constant time operations for map manipulations.
- **Space Complexity:** `O(n)`, since we maintain maps to record unique IDs for the subtrees.

