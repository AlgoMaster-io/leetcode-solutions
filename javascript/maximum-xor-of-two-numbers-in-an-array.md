## [Leetcode Problem 421: Maximum XOR of Two Numbers in an Array](https://leetcode.com/problems/maximum-xor-of-two-numbers-in-an-array/)

### Approaches:
1. [Brute Force Approach](#brute-force-approach)
2. [Trie Data Structure Approach](#trie-data-structure-approach)
3. [Optimal Bit Manipulation Approach](#optimal-bit-manipulation-approach)

---

### Brute Force Approach

#### Intuition:
The brute force approach involves checking each pair of numbers in the array and calculating the XOR value. We then track the maximum XOR value obtained. This method, although simple to understand and implement, does not scale well with large input sizes due to the high time complexity.

#### Implementation:

```javascript
var findMaximumXOR = function(nums) {
    let maxXOR = 0;
    const n = nums.length;
    
    // Check every pair (i, j) such that i < j
    for (let i = 0; i < n; i++) {
        for (let j = i + 1; j < n; j++) {
            // Calculate XOR between nums[i] and nums[j]
            const currentXOR = nums[i] ^ nums[j];
            // Update maxXOR if we find a new maximum
            maxXOR = Math.max(maxXOR, currentXOR);
        }
    }
    
    return maxXOR;
};
```

#### Time Complexity:
- O(n^2), where n is the number of elements in the array.
- We check XOR for each pair of elements in the array.

#### Space Complexity:
- O(1), since we only use a constant amount of extra space.

---

### Trie Data Structure Approach

#### Intuition:
A more efficient way leverages a Trie data structure to represent binary prefixes. The main idea is to build a Trie from the binary representation of the given numbers and attempt to find the best possible pair for maximizing XOR bit by bit. This allows us to efficiently track and compare useful prefixes as we fill in the Trie.

#### Implementation:

```javascript
class TrieNode {
    constructor() {
        // Initialize children for both bit 0 and bit 1
        this.children = {};
    }
}

var findMaximumXOR = function(nums) {
    const root = new TrieNode();
    
    const insert = (num) => {
        let node = root;
        // Insert the binary representation into the Trie
        for (let i = 31; i >= 0; i--) {  // A 32-bit integer
            const bit = (num >> i) & 1;
            if (!node.children[bit]) {
                node.children[bit] = new TrieNode();
            }
            node = node.children[bit];
        }
    };
    
    const findMaxXOR = (num) => {
        let node = root;
        let maxXOR = 0;
        // Try to find the maximum XOR for this particular num
        for (let i = 31; i >= 0; i--) {
            const bit = (num >> i) & 1;
            const toggledBit = 1 - bit;
            if (node.children[toggledBit]) {
                maxXOR = (maxXOR << 1) | 1;
                node = node.children[toggledBit];
            } else {
                maxXOR = maxXOR << 1;
                node = node.children[bit];
            }
        }
        return maxXOR;
    };
    
    for (const num of nums) {
        insert(num);
    }
    
    let max = 0;
    for (const num of nums) {
        max = Math.max(max, findMaxXOR(num));
    }
    
    return max;
};
```

#### Time Complexity:
- O(n * W), where n is the number of elements and W is the number of bits (32 for integer).
- Each number insertion and maximum XOR query are O(W).

#### Space Complexity:
- O(n * W), as the Trie might grow to have n nodes, each with depth W.

---

### Optimal Bit Manipulation Approach

#### Intuition:
The most optimal solution leverages bit manipulation and a greedy strategy. We iterate over the binary positions from high to low order bits, trying to deduce the largest XOR by building it one bit at a time.

#### Implementation:

```javascript
var findMaximumXOR = function(nums) {
    let maxResult = 0;
    let mask = 0;
    
    for (let i = 31; i >= 0; i--) {
        // Move the mask one position to the left
        mask = mask | (1 << i);
        
        const prefixes = new Set();
        for (const num of nums) {
            // Collect prefixes of the current number with respect to the current mask
            prefixes.add(num & mask);
        }
        
        // Temporarily set maxResult with the current bit set
        const tentativeMax = maxResult | (1 << i);
        
        for (const prefix of prefixes) {
            // Check if there is a pair of prefixes which XOR gives tentativeMax
            if (prefixes.has(prefix ^ tentativeMax)) {
                maxResult = tentativeMax;
                break;
            }
        }
    }
    
    return maxResult;
};
```

#### Time Complexity:
- O(n * W), where n is the number of elements and W (32 for integer).

#### Space Complexity:
- O(n), for storing unique prefixes.

This approach combines the insights from the previous methods and optimizes it to the fullest using bitwise operations, making it elegant and efficient.

