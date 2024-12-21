# [Leetcode 496: Next Greater Element I](https://leetcode.com/problems/next-greater-element-i/)

## Approaches:
1. [Brute Force Approach](#brute-force-approach)
2. [Using Stack and HashMap](#using-stack-and-hashmap)

### Brute Force Approach

#### Intuition:
In this approach, we iterate for each element in `nums1` through `nums2` to find the next greater element. For each element in `nums1`, we search for its position in `nums2`, and then continue traversing `nums2` from that position onward to find the next greater element. This method is straightforward but not the most efficient.

#### Steps:
1. Iterate through each element in `nums1`.
2. For each element, find its index in `nums2`.
3. From that index onward, find the next element in `nums2` that is greater.
4. If found, note it, otherwise note `-1`.

```javascript
function nextGreaterElement(nums1, nums2) {
    let result = [];
    
    // Iterate over each element in nums1
    for (let num1 of nums1) {
        let found = false;
        let index = nums2.indexOf(num1);  // Find the index of num1 in nums2
        
        // Scan elements in nums2 from the index onwards
        for (let j = index + 1; j < nums2.length; j++) {
            if (nums2[j] > num1) {  // If a greater element is found
                result.push(nums2[j]);
                found = true;
                break;
            }
        }
        
        // If no greater element is found, append -1
        if (!found) {
            result.push(-1);
        }
    }
    
    return result;
}
```

**Time Complexity**: O(n1 * n2), where n1 is the length of `nums1` and n2 is the length of `nums2`.

**Space Complexity**: O(1), except for the result storage.

### Using Stack and HashMap

#### Intuition:
The optimal solution leverages a stack to keep track of potential next greater elements and a hashmap to store the results for quick lookup. The stack helps us efficiently identify the next greater element by only maintaining a decreasing order, and the hashmap allows us to store each calculation result once, thus avoiding repeated work.

#### Steps:
1. Traverse `nums2` from the beginning to the end.
2. Use a stack to keep numbers in descending order.
3. While there's something in the stack and the current number is greater than the number on the top of the stack, pop from the stack. The popped elements' next greater element is the current number.
4. Store these results in a hashmap with the element as the key and the current number as its next greater element.
5. For each element in `nums1`, look up in the hashmap to get its next greater or default to `-1`.

```javascript
function nextGreaterElement(nums1, nums2) {
    let stack = [];
    let map = {};  // This will hold the next greater elements
    
    // Iterate over each number in nums2
    for (let num of nums2) {
        // Maintain the decreasing order in the stack
        while (stack.length && stack[stack.length - 1] < num) {
            map[stack.pop()] = num;
        }
        stack.push(num);
    }
    
    // Generate the result based on precomputed next greater elements
    return nums1.map(num => map[num] !== undefined ? map[num] : -1);
}
```

**Time Complexity**: O(n1 + n2), where n1 is the length of `nums1` and n2 is the length of `nums2`. We iterate through `nums2` once and derive results for `nums1` in a second pass.

**Space Complexity**: O(n2) for the stack and hashmap.

Through these approaches, we can solve the problem efficiently using both a basic method for understanding and a more optimal method utilizing data structures for better performance.

