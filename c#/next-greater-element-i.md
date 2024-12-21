# LeetCode Problem - Next Greater Element I

**Link to the problem:** [496. Next Greater Element I](https://leetcode.com/problems/next-greater-element-i/)

## Approaches
1. [Brute Force Approach](#brute-force-approach)
2. [Optimized Approach using Stack](#optimized-approach-using-stack)

---

### Brute Force Approach

The brute force solution involves iterating over `nums1` and, for each element, searching for the next greater element in `nums2`.

#### Intuition:
- For each element in `nums1`, locate the index of this element in `nums2`.
- Once found, iterate from that index onwards in `nums2` to search for the next greater element.
- If a next greater element is found, store it; otherwise, store `-1`.

```csharp
public int[] NextGreaterElement(int[] nums1, int[] nums2) {
    int[] result = new int[nums1.Length];
    
    for (int i = 0; i < nums1.Length; i++) {
        // Find the index of nums1[i] in nums2
        int j = 0;
        while (j < nums2.Length && nums2[j] != nums1[i]) {
            j++;
        }
        
        // Look for the next greater element in nums2
        int nextGreater = -1;
        for (int k = j + 1; k < nums2.Length; k++) {
            if (nums2[k] > nums1[i]) {
                nextGreater = nums2[k];
                break;
            }
        }
        
        result[i] = nextGreater;
    }
    
    return result;
}
```

**Time Complexity:** O(m * n) - where `m` is the length of `nums1` and `n` is the length of `nums2`.  
**Space Complexity:** O(1) - apart from the output array, no additional space is used.

---

### Optimized Approach using Stack

This approach uses a stack to efficiently find the next greater element for each number in `nums2` and utilizes a hashmap for fast look-up when answering queries in `nums1`.

#### Intuition:
- Traverse `nums2` from right to left.
- Use a stack to keep track of the next greater element seen so far.
- For each element, pop elements from the stack until the element in the stack is greater than the current element.
- Use a hashmap to map each element of `nums2` to its next greater element.
- Finally, for each element in `nums1`, use the hashmap to get the next greater element in `O(1)` time.

```csharp
public int[] NextGreaterElement(int[] nums1, int[] nums2) {
    // Dictionary to store the next greater element for each number in nums2
    Dictionary<int, int> nextGreaterMap = new Dictionary<int, int>();
    Stack<int> stack = new Stack<int>();
    
    // Traverse nums2 from right to left
    for (int i = nums2.Length - 1; i >= 0; i--) {
        // Remove elements from the stack that are less or equal to the current element
        while (stack.Count > 0 && stack.Peek() <= nums2[i]) {
            stack.Pop();
        }
        
        // If stack is empty, no next greater element
        // Otherwise, the top of the stack is the next greater element
        nextGreaterMap[nums2[i]] = stack.Count == 0 ? -1 : stack.Peek();
        
        // Push the current element onto the stack
        stack.Push(nums2[i]);
    }
    
    // Build the result for nums1 using the nextGreaterMap
    int[] result = new int[nums1.Length];
    for (int i = 0; i < nums1.Length; i++) {
        result[i] = nextGreaterMap[nums1[i]];
    }
    
    return result;
}
```

**Time Complexity:** O(n + m) - where `n` is the length of `nums2` and `m` is the number of queries in `nums1`.  
**Space Complexity:** O(n) - for storing the hashmap `nextGreaterMap`.

---

