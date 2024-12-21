# [Leetcode 421: Maximum XOR of Two Numbers in an Array](https://leetcode.com/problems/maximum-xor-of-two-numbers-in-an-array/)

In this problem, we are tasked to find two numbers in a given array such that the XOR of the two numbers is maximum. Here's a breakdown of multiple approaches to tackle this problem, from basic to the most optimal.

## Approaches:

- [Brute Force Solution](#brute-force-solution)
- [Optimal Solution using Trie](#optimal-solution-using-trie)

---

### Brute Force Solution

#### Intuition:

The simplest approach is to evaluate the XOR for each pair of numbers in the array and keep track of the maximum XOR encountered. This approach leverages the basic property of XOR operation to find the combination which yields the maximum value.

#### Solution:

```cpp
#include <vector>
#include <algorithm>

int findMaximumXOR(std::vector<int>& nums) {
    int maxXOR = 0; // Initialize maximum XOR as 0
    int n = nums.size();

    // Iterate over all pairs (i, j) to compute XOR
    for (int i = 0; i < n; ++i) {
        for (int j = i + 1; j < n; ++j) {
            // Compute XOR of nums[i] and nums[j]
            int xorVal = nums[i] ^ nums[j];
            // Update maximum XOR if current value is larger
            maxXOR = std::max(maxXOR, xorVal);
        }
    }
    
    return maxXOR;
}
```

#### Time Complexity: 
- **O(n^2)**, where n is the number of elements in the array. This is due to the nested loop which checks each pair.

#### Space Complexity:
- **O(1)**, as we are not using any additional data structures that grow with input size.

---

### Optimal Solution using Trie

#### Intuition:

To find the maximum XOR in an optimized way, we can use a Trie (prefix tree). The idea is to insert all numbers in binary format into the Trie and then find two numbers that result in the maximum XOR by taking advantage of the binary trie structure.

By using a trie, you can keep track of the best possible complement for every bit starting from the most significant bit to the least significant one, which allows us to calculate the maximum XOR efficiently.

#### Solution:

```cpp
#include <vector>
#include <iostream>
#include <bitset>

class TrieNode {
public:
    TrieNode* left;  // to store 0
    TrieNode* right; // to store 1

    TrieNode() : left(nullptr), right(nullptr) {}
};

class Trie {
public:
    TrieNode* root;
    
    Trie() {
        root = new TrieNode();
    }
    
    void insert(int num) {
        TrieNode* curr = root;
        // Iterate over each bit from most significant to least significant
        for (int i = 31; i >= 0; --i) {
            int bit = (num >> i) & 1;
            if (bit == 0) {
                if (!curr->left) {
                    curr->left = new TrieNode(); // create a new node if the path for bit 0 doesn't exist
                }
                curr = curr->left; // move to the path for bit 0
            } else {
                if (!curr->right) {
                    curr->right = new TrieNode(); // create a new node if the path for bit 1 doesn't exist
                }
                curr = curr->right; // move to the path for bit 1
            }
        }
    }
    
    int findMaxXOR(int num) {
        TrieNode* curr = root;
        int xorNum = 0;
        // Iterate over each bit from most significant to least significant
        for (int i = 31; i >= 0; --i) {
            int bit = (num >> i) & 1;
            if (bit == 0) {
                if (curr->right) {
                    xorNum |= (1 << i); // move to a 1-bit to maximize XOR
                    curr = curr->right;
                } else {
                    curr = curr->left;
                }
            } else {
                if (curr->left) {
                    xorNum |= (1 << i); // move to a 0-bit to maximize XOR
                    curr = curr->left;
                } else {
                    curr = curr->right;
                }
            }
        }
        return xorNum;
    }
};

int findMaximumXOR(std::vector<int>& nums) {
    Trie trie;
    for (int num : nums) {
        trie.insert(num);
    }

    int maxXOR = 0;
    for (int num : nums) {
        maxXOR = std::max(maxXOR, trie.findMaxXOR(num));
    }

    return maxXOR;
}
```

#### Time Complexity:
- **O(n)** for inserting elements into the Trie, and **O(n\*m)** for finding the maximum XOR for each number where m is the number of bits (32 for a standard integer).

#### Space Complexity:
- **O(n\*m)**, where n is the number of integers and m is the number of bits used (32 here). This space is used for storing the trie.

In this optimal solution using a Trie, we efficiently find the maximum XOR by storing binary representations and evaluating potential maximum complements for each bit position. This approach greatly reduces the number of calculations required compared to the brute force method.

