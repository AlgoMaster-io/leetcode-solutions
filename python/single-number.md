[LeetCode Problem 136: Single Number](https://leetcode.com/problems/single-number/)

## Approaches
- [Approach 1: Using a Dictionary or Hash Map](#approach-1-using-a-dictionary-or-hash-map)
- [Approach 2: Using Sorting](#approach-2-using-sorting)
- [Approach 3: Using XOR](#approach-3-using-xor)

### Approach 1: Using a Dictionary or Hash Map

#### Intuition
The main idea in this approach is to use a dictionary (or hash map) to count the occurrences of each number. In this problem, every number appears twice except for one. Hence, after counting the occurrences, we can easily find the number that occurs once.

#### Solution

```python
def singleNumber(nums):
    # Create a dictionary to hold the count of each number
    num_count = {}
    
    # Count occurrences of each number
    for num in nums:
        if num in num_count:
            num_count[num] += 1
        else:
            num_count[num] = 1
    
    # Iterate through the dictionary to find the single number
    for num, count in num_count.items():
        if count == 1:
            return num

# Example usage
nums = [4, 1, 2, 1, 2]
print(singleNumber(nums))  # Output is 4
```

**Time Complexity**: O(n), where n is the number of elements in the list, as we traverse the array once to populate the dictionary.

**Space Complexity**: O(n), as we store each number in a dictionary.

### Approach 2: Using Sorting

#### Intuition
If the array were sorted, the single number would be the only number without a matching pair adjacent to it. Thus, we can first sort the array and then look for this unpaired number.

#### Solution

```python
def singleNumber(nums):
    # Sort the array
    nums.sort()
    
    # Check pairs of numbers
    i = 0
    while i < len(nums) - 1:
        # If the current number and the next number aren't the same, return the current number
        if nums[i] != nums[i + 1]:
            return nums[i]
        # Move the index ahead by 2 (because we expect pairs)
        i += 2
    
    # Return the last number if no unpaired found till the last pair
    return nums[-1]

# Example usage
nums = [4, 1, 2, 1, 2]
print(singleNumber(nums))  # Output is 4
```

**Time Complexity**: O(n log n), due to the sorting step.

**Space Complexity**: O(1), no extra space is used except for the input.

### Approach 3: Using XOR

#### Intuition
The XOR operation has a property where `a ⊕ a = 0` and `a ⊕ 0 = a`. Thus, XOR-ing all numbers in the array will cancel out numbers appearing twice, leaving the single number.

#### Solution

```python
def singleNumber(nums):
    # Initialize a variable to hold the XOR result
    result = 0
    
    # XOR all the elements in the array
    for num in nums:
        result ^= num
    
    # The result holds the single number now
    return result

# Example usage
nums = [4, 1, 2, 1, 2]
print(singleNumber(nums))  # Output is 4
```

**Time Complexity**: O(n), as we traverse the list once.

**Space Complexity**: O(1), as no additional space is used.

