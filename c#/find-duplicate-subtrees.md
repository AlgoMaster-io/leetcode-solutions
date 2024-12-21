## [Leetcode 652: Find Duplicate Subtrees](https://leetcode.com/problems/find-duplicate-subtrees/)

### Table of Contents
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Using Subtree Serialization and HashMap](#approach-2-using-subtree-serialization-and-hashmap)

---

### Approach 1: Brute Force

#### Intuition:
The brute force approach involves traversing through each subtree and comparing it with every other subtree to check if they are identical. This is not efficient due to multiple redundant comparisons.

#### Steps:
1. For each node, construct its subtree representation.
2. Compare this subtree with all other subtrees.
3. If two subtree representations are identical, it's a duplicate and should be recorded.

#### Code:
```csharp
public class Solution 
{
    public IList<TreeNode> FindDuplicateSubtrees(TreeNode root) 
    {
        List<TreeNode> duplicates = new List<TreeNode>();
        List<string> subtreeList = new List<string>();
        
        void InOrderTraverse(TreeNode node)
        {
            if (node == null) return;
            
            string leftSubtree = node.left != null ? Serialize(node.left) : "#";
            string rightSubtree = node.right != null ? Serialize(node.right) : "#";
            
            string currentSubtree = $"{node.val},{leftSubtree},{rightSubtree}";
            
            // Check if we have already encountered this subtree
            if (subtreeList.Contains(currentSubtree))
            {
                duplicates.Add(node);
            }
            else
            {
                subtreeList.Add(currentSubtree);
            }
            
            InOrderTraverse(node.left);
            InOrderTraverse(node.right);
        }
        
        string Serialize(TreeNode node)
        {
            if (node == null) return "#";
            return $"{node.val},{Serialize(node.left)},{Serialize(node.right)}";
        }
        
        InOrderTraverse(root);
        return duplicates;
    }
}
```

#### Complexity:
- **Time complexity:** O(n^2), where n is the number of nodes. Each subtree is compared with others resulting in n^2 comparisons in the worst case.
- **Space complexity:** O(n), for storing each subtree representation.

---

### Approach 2: Using Subtree Serialization and HashMap

#### Intuition:
To optimize the solution, we can use serialization of the subtrees with hashmaps to check for the duplicates efficiently. We serialize each subtree to a string and keep track of its occurrence in a hashmap. If a subtree's serialization is found more than once, it indicates a duplicate.

#### Steps:
1. Traverse the tree in a post-order fashion.
2. For each node, serialize the subtree rooted at the node.
3. Store this serialized form in a hashmap, counting its occurrences.
4. If a serialized form appears more than once, add the corresponding node to the duplicates list.

#### Code:
```csharp
public class Solution
{
    public IList<TreeNode> FindDuplicateSubtrees(TreeNode root) 
    {
        Dictionary<string, int> subtreeMap = new Dictionary<string, int>();
        List<TreeNode> duplicates = new List<TreeNode>();
        SerializeAndFind(root, subtreeMap, duplicates);
        return duplicates;
    }
    
    private string SerializeAndFind(TreeNode node, Dictionary<string, int> map, List<TreeNode> duplicates) 
    {
        if (node == null) return "#";
        
        // Serialize the current subtree
        string serialized = $"{node.val},{SerializeAndFind(node.left, map, duplicates)},{SerializeAndFind(node.right, map, duplicates)}";
        
        // Count occurrences of this serialized form
        if (map.ContainsKey(serialized))
        {
            map[serialized]++;
        }
        else
        {
            map[serialized] = 1;
        }
        
        // Only add to duplicates if this is exactly the second time we encounter this subtree pattern
        if (map[serialized] == 2) 
        {
            duplicates.Add(node);
        }
        
        return serialized;
    }
}
```

#### Complexity:
- **Time complexity:** O(n), as each node is processed once.
- **Space complexity:** O(n), for storing serialized subtrees and the duplicates list.

By using hashmap to identify duplicate subtrees, this approach efficiently finds duplicates without excessive comparisons.

