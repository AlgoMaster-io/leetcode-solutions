# [Leetcode 496: Next Greater Element I](https://leetcode.com/problems/next-greater-element-i/)

## Approaches
1. [Basic Brute Force Approach](#approach-1-basic-brute-force-approach)
2. [Optimized Approach Using a Stack and Hash Map](#approach-2-optimized-approach-using-a-stack-and-hash-map)

### Approach 1: Basic Brute Force Approach

#### Intuition:
In this approach, for each element in `nums1`, we need to find the next greater element in `nums2`. We can simply loop through `nums1`, and for each element, search for it in `nums2`. Once found, continue scanning the rest of `nums2` to find the next greater element. If no such element exists, return -1.

#### Detailed Steps:
- Initialize an empty array `res` to store the results.
- For each element in `nums1`, find its index in `nums2`.
- From this location in `nums2`, check each subsequent element to find a greater one.
- Append the first greater element found to `res` or append -1 if there is no greater element.

```python
def nextGreaterElement(nums1, nums2):
    res = []
    for num in nums1:
        found = False
        index = nums2.index(num)  # Find the index of num in nums2
        # Continue from the next element in nums2
        for next_num in nums2[index + 1:]:
            if next_num > num:
                res.append(next_num)
                found = True
                break
        if not found:
            res.append(-1)
    return res
```

#### Time Complexity:
- **O(m * n)** where m is the length of `nums1` and n is the length of `nums2`. This is because for each number in `nums1`, we may need to scan the entire `nums2` array.

#### Space Complexity:
- **O(1)**, apart from the output list, as no additional data structures are used.

### Approach 2: Optimized Approach Using a Stack and Hash Map

#### Intuition:
We can improve upon the brute-force idea by leveraging a stack data structure. This allows us to efficiently find the next greater element in a single pass through `nums2`. The key idea is to maintain a decreasing stack. Whenever an element larger than the top of the stack is encountered, this element is the next greater element for the one at the top of the stack.

#### Detailed Steps:
1. Use a stack to keep track of elements for which we haven't found a next greater element.
2. Use a dictionary (hash map) to map each element to its next greater element.
3. Iterate over `nums2`. For each element, resolve the next greater for elements in the stack.
4. For each element in `nums1`, use the dictionary to find the pre-computed next greater element.

```python
def nextGreaterElement(nums1, nums2):
    stack = []  # Use stack to keep potential candidates
    next_greater_map = {}  # HashMap to store the next greater element for each num

    for num in nums2:
        # While there is a smaller number on top of the stack, this num is its next greater
        while stack and stack[-1] < num:
            smaller_num = stack.pop()
            next_greater_map[smaller_num] = num
        stack.append(num)

    # If some numbers are left, they don't have a next greater, so map them to -1
    while stack:
        no_next_greater = stack.pop()
        next_greater_map[no_next_greater] = -1

    # Create the result by mapping each num in nums1 to their next greater in nums2
    return [next_greater_map[num] for num in nums1]
```

#### Time Complexity:
- **O(n)**: where n is the length of `nums2`. The `while` loop ensures each element is pushed and popped at most once.

#### Space Complexity:
- **O(n)**: extra space used by the dictionary `next_greater_map` and the stack, both of which store elements of `nums2`.

