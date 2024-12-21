[Leetcode Problem 260: Single Number III](https://leetcode.com/problems/single-number-iii/)

### Approaches:
1. [Using HashMap](#using-hashmap)
2. [Using Bit Manipulation](#using-bit-manipulation)

---

## Using HashMap

### Intuition
We can use a hashmap to keep track of the frequency of each number. By the end of iterating through the list, any number that appears only once will have a frequency of one. This approach leverages the fact that numbers other than the two unique numbers appear twice.

### Approach
1. Initialize an empty hashmap `frequency`.
2. Iterate over each number in the list:
   - If the number exists in the hashmap, increment its count.
   - If not, add the number to the hashmap with a count of 1.
3. At the end of the iteration, extract numbers from the hashmap that have a count of 1.
4. Return these numbers.

### Implementation

```python
def singleNumber(nums):
    # Step 1: Initialize a hashmap to store frequency of each number
    frequency = {}
    
    # Step 2: Populate the hashmap with the frequency of each number
    for num in nums:
        frequency[num] = frequency.get(num, 0) + 1
    
    # Step 3: Collect numbers that appear only once
    result = [num for num, count in frequency.items() if count == 1]
    
    # Step 4: Return the result
    return result

# Example usage:
# Input: nums = [1,2,1,3,2,5]
# Output: [3,5]
```

### Complexity Analysis
- **Time Complexity**: O(n), where n is the length of the input list, as we iterate the list twice (once to populate the hashmap, and once to filter results).
- **Space Complexity**: O(n), as we store all numbers and their frequencies in the hashmap.

---

## Using Bit Manipulation

### Intuition
To find two unique numbers using bitwise operations, we can leverage XOR's properties. XOR of two same numbers is 0; thus XOR all numbers result in the XOR of the two unique numbers. The main challenge is to then split the numbers into two groups, so each group contains one of the unique numbers. We can differentiate the numbers by finding a set bit in the XOR result, which indicates the positions where the two numbers differ.

### Approach
1. XOR all numbers. Let this result be `xor`, which is equal to the XOR of the two unique numbers.
2. Find any bit position where `xor` has a `1`. This indicates that the two unique numbers differ at that bit position.
3. Partition the numbers into two groups based on whether they have a `1` or `0` at the set bit and XOR within each group.
4. The result will be the two unique numbers.

### Implementation

```python
def singleNumber(nums):
    # Step 1: XOR all numbers to get xor of the two unique numbers
    xor = 0
    for num in nums:
        xor ^= num
    
    # Step 2: Find a set bit in xor (this bit is set in one unique number and not in another)
    diff_bit = xor & -xor
    
    # Step 3: Initialize the results for the two unique numbers
    result1 = 0
    result2 = 0
    
    # Step 4: Partition numbers into two groups and XOR each group
    for num in nums:
        if num & diff_bit:
            result1 ^= num
        else:
            result2 ^= num
    
    # Step 5: Return the two unique numbers
    return [result1, result2]

# Example usage:
# Input: nums = [1,2,1,3,2,5]
# Output: [3,5]
```

### Complexity Analysis
- **Time Complexity**: O(n), where n is the length of the input list, as we iterate through the list mainly twice.
- **Space Complexity**: O(1), since we are using a fixed amount of extra space regardless of input size.

