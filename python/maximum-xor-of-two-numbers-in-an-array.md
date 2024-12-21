# [Leetcode 421: Maximum XOR of Two Numbers in an Array](https://leetcode.com/problems/maximum-xor-of-two-numbers-in-an-array/)

## Approaches
- [Brute Force Approach](#brute-force-approach)
- [Trie Based Optimization](#trie-based-optimization)

### Brute Force Approach

#### Intuition:
The brute force approach entails calculating the XOR for each possible pair of numbers in the given array and keeping track of the maximum XOR value encountered. Given the basic property of XOR where `a XOR b` is maximized when the bits of `a` and `b` are as different as possible, we simply need to iterate over all pairs and compute the XOR.

#### Detailed Steps:
1. Initialize a variable `max_xor` to 0 which will hold the maximum XOR value found.
2. Use nested loops to iterate over all possible pairs `(nums[i], nums[j])` where `0 <= i < j < len(nums)`.
3. Compute `xor_value = nums[i] ^ nums[j]`.
4. Update `max_xor` with `max(max_xor, xor_value)`.
5. After exploring all pairs, `max_xor` will hold the maximum XOR value.

#### Time Complexity:
- **O(n^2)**: We are considering all possible pairs, hence quadratic time complexity.

#### Space Complexity:
- **O(1)**: We use only a constant amount of extra space.

```python
def findMaximumXOR(nums):
    max_xor = 0
    # iterate over all possible pairs
    for i in range(len(nums)):
        for j in range(i+1, len(nums)):
            # calculate XOR of the pair and update max_xor
            max_xor = max(max_xor, nums[i] ^ nums[j])
    return max_xor
```

### Trie Based Optimization

#### Intuition:
To optimize, we can leverage the properties of a binary trie. The key idea here is that the bits in binary numbers can guide us to form the largest possible XOR. We can insert each number's binary representation into a trie and then for each number, attempt to find its XOR complement that maximizes the XOR result.

#### Detailed Steps:
1. Define a `TrieNode` class with references to two children, representing the binary `0` and `1`.
2. Build a trie from the binary representations of the numbers in the array.
3. For each number, traverse the trie trying to find the opposite bit (to maximize the XOR). If a complementary bit exists, that path is taken.
4. Keep track of the maximum XOR obtained through these traversals.

#### Time Complexity:
- **O(n * L)**: Constructing the trie and computing XOR, where `n` is the number of elements and `L` is the maximum length of the binary representation (which is 32 for integers).

#### Space Complexity:
- **O(n * L)**: Space required to store the trie.

```python
class TrieNode:
    def __init__(self):
        self.zero = None
        self.one = None

def findMaximumXOR(nums):
    # Create root of Trie
    root = TrieNode()
    L = len(bin(max(nums))) - 2  # Find the maximum number of bits needed by the greatest number
    # Build a Trie
    for num in nums:
        node = root
        for i in range(L, -1, -1):
            bit = (num >> i) & 1
            if bit == 0:
                if not node.zero:
                    node.zero = TrieNode()
                node = node.zero
            else:
                if not node.one:
                    node.one = TrieNode()
                node = node.one

    max_xor = 0
    # Find max XOR
    for num in nums:
        node = root
        curr_xor = 0
        for i in range(L, -1, -1):
            bit = (num >> i) & 1
            # Try to follow the opposite bit to maximize XOR
            if bit == 0:
                if node.one:
                    curr_xor = (curr_xor << 1) | 1
                    node = node.one
                else:
                    curr_xor = curr_xor << 1
                    node = node.zero
            else:
                if node.zero:
                    curr_xor = (curr_xor << 1) | 1
                    node = node.zero
                else:
                    curr_xor = curr_xor << 1
                    node = node.one
        max_xor = max(max_xor, curr_xor)
    return max_xor
```
This trie-based solution is much more optimal than the brute force approach, particularly for large inputs, due to its linear dependence on the size of the input array.

