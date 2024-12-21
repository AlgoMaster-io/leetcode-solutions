# [Leetcode 421: Maximum XOR of Two Numbers in an Array](https://leetcode.com/problems/maximum-xor-of-two-numbers-in-an-array/)

## Table of Contents
- [Brute Force Approach](#brute-force-approach)
- [Optimal Approach Using Trie](#optimal-approach-using-trie)

## Brute Force Approach

### Intuition
For the brute force method, we consider each possible pair of numbers in the array and calculate their XOR. The XOR operation will give us a number with bits set where the bits differ between the two numbers. Our task is to find the two numbers that maximize this XOR. This approach checks all pairs and picks the one with the maximum XOR value.

### Algorithm
1. Initialize a variable `maxXOR` to zero.
2. For every pair of numbers `(A[i], A[j])` in the array, compute the XOR `A[i] ^ A[j]`.
3. Update `maxXOR` if the computed XOR is greater than `maxXOR`.
4. Return `maxXOR`.

### Java Code
```java
public int findMaximumXOR(int[] nums) {
    int maxXOR = 0;
    for (int i = 0; i < nums.length; i++) {
        for (int j = i + 1; j < nums.length; j++) {
            // Calculate the XOR for this pair
            int currentXOR = nums[i] ^ nums[j];
            // Update maxXOR if the currentXOR is larger
            maxXOR = Math.max(maxXOR, currentXOR);
        }
    }
    return maxXOR;
}
```

### Time Complexity
- **O(n^2)**: We iterate over all possible pairs in the array.
 
### Space Complexity
- **O(1)**: No additional space is used except for a few variables.

## Optimal Approach Using Trie

### Intuition
The key idea is to use a Trie (prefix tree) to efficiently find two numbers in the array that can form the maximum XOR result. By inserting all numbers in the Trie and using bit manipulation, we can find pairs leading to maximum XOR efficiently. This relies on the fact that XOR tends to maximize when we have opposing bits (i.e., one bit is `0`, the other is `1`).

### Algorithm
1. Define a Trie structure with only two children for each node, representing a bit of `0` or `1`.
2. Insert each number in the array into the Trie.
3. As each number is inserted, simultaneously calculate the maximum XOR for that number with the previously inserted numbers in the Trie.
4. Return the highest XOR obtained.

### Java Code
```java
class TrieNode {
    TrieNode[] children = new TrieNode[2];
}

public int findMaximumXOR(int[] nums) {
    TrieNode root = new TrieNode();
    int maxXOR = 0;

    for (int num : nums) {
        TrieNode current = root, complement = root;
        int currentXOR = 0;
        for (int i = 31; i >= 0; i--) {
            int bit = (num >> i) & 1;
            // Insert bit in Trie
            if (current.children[bit] == null) {
                current.children[bit] = new TrieNode();
            }
            current = current.children[bit];
            
            // Search for complement bit
            int toggledBit = bit ^ 1;
            if (complement.children[toggledBit] != null) {
                currentXOR = (currentXOR << 1) | 1;
                complement = complement.children[toggledBit];
            } else {
                currentXOR = (currentXOR << 1);
                complement = complement.children[bit];
            }
        }
        // Update maxXOR with the current number's optimum XOR
        maxXOR = Math.max(maxXOR, currentXOR);
    }

    return maxXOR;
}
```

### Time Complexity
- **O(n * 32)**: Each number is pushed into the Trie in O(32) time complexity, equivalent to the number of bits in an integer, and we process each number.

### Space Complexity
- **O(n * 32)**: Space used in the Trie for storing all n numbers, each represented by a sequence of 32 bits.

