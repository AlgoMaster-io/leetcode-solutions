# [Leetcode 496: Next Greater Element I](https://leetcode.com/problems/next-greater-element-i/)

## Approaches:
1. [Brute Force](#brute-force)
2. [Using a Stack and HashMap](#using-a-stack-and-hashmap)

---

### Brute Force

The brute force approach is straightforward: For each element in `nums1`, search for its next greater element in `nums2`.

#### Intuition
- For each element in `nums1`, iterate over `nums2` to find the index of that element.
- From that index, search for the first element greater than the current element.
- If a greater element is found, add it to the result, otherwise, append -1 (indicating no greater element found).

#### Complexity
- **Time Complexity:** O(n * m), where n is the length of `nums1` and m is the length of `nums2`. For each element in `nums1`, we do a potentially full scan of `nums2`.
- **Space Complexity:** O(1), apart from the space used to store the result.

```java
public int[] nextGreaterElement(int[] nums1, int[] nums2) {
    int[] result = new int[nums1.length];
    for (int i = 0; i < nums1.length; i++) {
        // Assume no greater element exists
        result[i] = -1;
        // Find the current element in nums2
        boolean found = false;
        for (int j = 0; j < nums2.length; j++) {
            if (nums2[j] == nums1[i]) {
                found = true;
            }
            // If the number is found, find the next greater element
            if (found && nums2[j] > nums1[i]) {
                result[i] = nums2[j];
                break;
            }
        }
    }
    return result;
}
```

---

### Using a Stack and HashMap

This approach leverages a stack and hashmap to efficiently find the next greater element for all elements in `nums2` in a single pass.

#### Intuition
- Use a stack to track elements for which we are finding the next greater element.
- As we iterate `nums2`, for each element, pop elements from the stack if the current element is greater because the current element is the "next greater" for those popped elements.
- Map each popped element to the current element in a hashmap.
- After processing, for each element in `nums1`, retrieve the next greater element directly from the hashmap.

#### Complexity
- **Time Complexity:** O(n + m), iterating nums2 once O(m), and checking each nums1 element in O(1) using the hashmap.
- **Space Complexity:** O(m), for storing the hashmap and the stack.

```java
public int[] nextGreaterElement(int[] nums1, int[] nums2) {
    Map<Integer, Integer> map = new HashMap<>();
    Stack<Integer> stack = new Stack<>();

    // Iterate over nums2
    for (int num : nums2) {
        // While stack is not empty and current num is greater than stack's top element
        while (!stack.isEmpty() && stack.peek() < num) {
            map.put(stack.pop(), num);  // map the last element to the current as its next greater
        }
        stack.push(num); // Push current element
    }
    
    // For the remaining elements in stack, no greater element found, thus they map to -1 by default

    int[] result = new int[nums1.length];
    for (int i = 0; i < nums1.length; i++) {
        result[i] = map.getOrDefault(nums1[i], -1);  // Retrieve next greater element from the map
    }
    return result;
}
```

This stack and hashmap approach is optimal for solving the Next Greater Element I problem and provides efficient and clear mapping of the elements in `nums1` to their next greater elements in `nums2`.

