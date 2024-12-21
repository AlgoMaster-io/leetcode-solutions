# [Leetcode 496: Next Greater Element I](https://leetcode.com/problems/next-greater-element-i/)

## Solutions

- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Using Stack and Hash Map](#approach-2-using-stack-and-hash-map)

---

### Approach 1: Brute Force 

**Intuition:**

The brute force solution involves iterating over each element in `nums1` and searching for the next greater element in `nums2`. For each element in `nums1`, we locate its position in `nums2` and search forward until we find a greater element.

**Algorithm:**

1. Iterate over each element, `x`, in `nums1`.
2. For each `x`, find its index in `nums2`.
3. From this index, iterate through the rest of `nums2` looking for the first element greater than `x`.
4. If found, this is the next greater element; otherwise, the result is `-1`.

**Code:**

```cpp
class Solution {
public:
    vector<int> nextGreaterElement(vector<int>& nums1, vector<int>& nums2) {
        vector<int> result;
        
        // For each element in nums1
        for (int x : nums1) {
            // Find the position of x in nums2
            auto it = find(nums2.begin(), nums2.end(), x);
            bool found = false;
            
            // From this position onwards, find the next greater element
            for (auto iter = it; iter != nums2.end(); ++iter) {
                if (*iter > x) {
                    result.push_back(*iter);
                    found = true;
                    break;
                }
            }
            
            // If no greater element is found, push -1
            if (!found) result.push_back(-1);
        }
        
        return result;
    }
};
```

**Complexity Analysis:**

- **Time Complexity:** O(n1 * n2), where `n1` is the size of `nums1` and `n2` is the size of `nums2`. This is because for each element in `nums1`, we may traverse all elements in `nums2`.
- **Space Complexity:** O(1), if we disregard the output space because no extra space is used apart from the result vector.

---

### Approach 2: Using Stack and Hash Map

**Intuition:**

An efficient way to solve this problem is to use a monotonic stack that helps in processing each element in `nums2` in a single pass, while maintaining a mapping from elements to their next greater element using a hash map. The stack is maintained such that elements are in decreasing order from the bottom to the top.

**Algorithm:**

1. Traverse `nums2` from left to right.
2. Use a stack to keep track of previously scanned numbers that haven't found their next greater element yet.
3. For each number in `nums2`, pop elements from the stack until the current number is smaller than the stack's top element or the stack is empty, and store each popped element's next greater element in the hash map.
4. Push the current number onto the stack.
5. Finally, construct the result for `nums1` using the computed map.

**Code:**

```cpp
class Solution {
public:
    vector<int> nextGreaterElement(vector<int>& nums1, vector<int>& nums2) {
        unordered_map<int, int> nge_map; // Hash map to store next greater element.
        stack<int> s; // Stack to keep track of decreasing numbers.
        
        // Iterate through nums2 to populate the hash map.
        for (int num : nums2) {
            // If the current number is greater than the number on top of the stack,
            // It is the next greater element of the stack's top number.
            while (!s.empty() && num > s.top()) {
                nge_map[s.top()] = num;
                s.pop();
            }
            // Push the current number onto the stack.
            s.push(num);
        }
        
        // Generate the result for nums1 based on the nge_map
        vector<int> result;
        for (int num : nums1) {
            result.push_back(nge_map.count(num) ? nge_map[num] : -1);
        }
        
        return result;
    }
};
```

**Complexity Analysis:**

- **Time Complexity:** O(n2 + n1). We process each element of `nums2` once due to the stack operations, and then we simply look up each element in `nums1` in the hash map.
- **Space Complexity:** O(n2). The stack can at most hold all elements of `nums2`, and we also store all elements in the hash map.

