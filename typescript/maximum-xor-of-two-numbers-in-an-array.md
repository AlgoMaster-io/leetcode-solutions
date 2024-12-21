# [Leetcode 421: Maximum XOR of Two Numbers in an Array](https://leetcode.com/problems/maximum-xor-of-two-numbers-in-an-array/)

### Approaches
- [Brute Force Approach](#brute-force-approach)
- [Optimized Approach Using Trie](#optimized-approach-using-trie)

## Brute Force Approach

### Intuition
The simplest way to solve this problem is to compare every possible pair of numbers. Calculate the XOR for each pair and keep track of the maximum XOR value obtained. This is an exhaustive search approach but can be useful to understand the basic mechanics of XOR.

### Implementation
```typescript
function findMaximumXOR(nums: number[]): number {
    let maxXor = 0;
    const n = nums.length;

    // Compare each pair of numbers to find the maximum XOR
    for (let i = 0; i < n; i++) {
        for (let j = i + 1; j < n; j++) {
            // Compute XOR and update maxXor if current XOR is larger
            const currentXor = nums[i] ^ nums[j];
            maxXor = Math.max(maxXor, currentXor);
        }
    }
    
    return maxXor;
}
```

### Time Complexity
- **Time Complexity**: O(n^2) - The function explores each pair of elements in the array.
  
### Space Complexity
- **Space Complexity**: O(1) - No additional space is required aside from input and control variables.

---

## Optimized Approach Using Trie

### Intuition
A more efficient way to solve this problem is by using a Trie data structure. The idea is to insert each number into a trie, one bit at a time. As we try to build up the maximum XOR, we can choose paths in the trie that maximize the XOR value. The key here is to look for the opposite bit (1 vs 0) in the trie as we traverse from the most significant bit to the least to maximize XOR for each bit position.

### Implementation
```typescript
class TrieNode {
    children: { [key: number]: TrieNode } = {};
}

function findMaximumXOR(nums: number[]): number {
    let root = new TrieNode();
    
    // Function to insert a number into the Trie
    const insert = (num: number) => {
        let node = root;
        for (let i = 31; i >= 0; i--) {
            const bit = (num >> i) & 1;
            if (!(bit in node.children)) {
                node.children[bit] = new TrieNode();
            }
            node = node.children[bit];
        }
    };

    // Function to find maximum XOR for a number with the existing numbers in Trie
    const findMax = (num: number): number => {
        let node = root;
        let maxXor = 0;
        for (let i = 31; i >= 0; i--) {
            const bit = (num >> i) & 1;
            // Try to take the opposite bit if possible for maximizing XOR
            const oppositeBit = 1 - bit;
            if (oppositeBit in node.children) {
                maxXor = (maxXor << 1) | 1;
                node = node.children[oppositeBit];
            } else {
                maxXor = (maxXor << 1) | 0;
                node = node.children[bit];
            }
        }
        return maxXor;
    };

    let maxResult = 0;
    
    for (const num of nums) {
        insert(num); // Insert into Trie
    }
    
    for (const num of nums) {
        maxResult = Math.max(maxResult, findMax(num)); // Find maximal XOR for each number
    }
    
    return maxResult;
}
```

### Time Complexity
- **Time Complexity**: O(n) - Each insertion and lookup in the Trie takes constant time proportional to the number of bits, and we perform it for each number in the list.

### Space Complexity
- **Space Complexity**: O(n) - We store up to n numbers in the Trie, yielding maximum tree nodes relative to the number of unique paths needed.

This Trie-based approach significantly optimizes the resolution of this problem and scales well with large input sizes compared to the brute force method.

