## [Leetcode 421: Maximum XOR of Two Numbers in an Array](https://leetcode.com/problems/maximum-xor-of-two-numbers-in-an-array/)

### Approaches
- [Brute Force Approach](#brute-force-approach)
- [Trie-Based Approach](#trie-based-approach)

---

### Brute Force Approach

The brute force approach involves iterating through all possible pairs of numbers in the array and computing their XOR. We keep track of the maximum XOR encountered. 

#### Intuition:
- XOR of two numbers will give us a value that reflects differing bits in those numbers.
- By evaluating all possible pairs, we can ensure that we have the maximum XOR combination.

#### Code:

```csharp
public int FindMaximumXOR(int[] nums) {
    int maxXOR = 0;
    // Iterate through each possible pair of numbers
    for (int i = 0; i < nums.Length; i++) {
        for (int j = i + 1; j < nums.Length; j++) {
            // Calculate XOR for the current pair
            int xor = nums[i] ^ nums[j];
            // Update max XOR if current XOR is greater
            if (xor > maxXOR) {
                maxXOR = xor;
            }
        }
    }
    return maxXOR;
}
```

#### Time Complexity:
- O(n^2): We evaluate each pair of numbers, leading to a quadratic complexity.

#### Space Complexity:
- O(1): We use a constant amount of extra space.

---

### Trie-Based Approach

This approach uses a binary trie to efficiently evaluate the maximum XOR by leveraging the properties of bits and their complements.

#### Intuition:
- A Trie can represent binary numbers efficiently with each level corresponding to a bit position (0 or 1).
- While inserting numbers into the Trie, always attempt to take the opposite direction (0 vs 1) to maximize XOR.
- The idea is to maximize the number of differing bits for all pairs by examining each bit position and its complement.

#### Code:

```csharp
public int FindMaximumXOR(int[] nums) {
    // TrieNode definition
    class TrieNode {
        public TrieNode[] children;
        public TrieNode() {
            children = new TrieNode[2];
        }
    }

    TrieNode root = new TrieNode();
    
    // Insert number into trie
    void Insert(int num) {
        TrieNode node = root;
        // Go through each bit position, from MSB to LSB
        for (int i = 31; i >= 0; i--) {
            // Check whether the ith bit is 1 or 0
            int bit = (num >> i) & 1;
            // If the desired path is not available, create a new TrieNode
            if (node.children[bit] == null) {
                node.children[bit] = new TrieNode();
            }
            // Move to the next node
            node = node.children[bit];
        }
    }
    
    // Find max xor for the given number with the trie
    int MaxXor(int num) {
        TrieNode node = root;
        int xor = 0;
        // Go through each bit position
        for (int i = 31; i >= 0; i--) {
            int bit = (num >> i) & 1;
            // Try to go in the opposite direction for maximum XOR
            int oppositeBit = 1 - bit;
            if (node.children[oppositeBit] != null) {
                xor |= (1 << i); // Set the ith bit in result
                node = node.children[oppositeBit];
            } else {
                node = node.children[bit];
            }
        }
        return xor;
    }
    
    int maxXOR = 0;
    // Insert first number into trie, then from the second number onward, calculate XOR
    foreach (int num in nums) {
        Insert(num);
        maxXOR = Math.Max(maxXOR, MaxXor(num));
    }
    
    return maxXOR;
}
```

#### Time Complexity:
- O(n * L): Where L represents the number of bits to store numbers (for this problem, L is typically 32).

#### Space Complexity:
- O(n * L): Space needed for Trie storage based on the number of elements and their bit representation.

