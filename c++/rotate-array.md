# [189. Rotate Array](https://leetcode.com/problems/rotate-array/)

## Approaches:
- [Approach 1: Brute Force Rotation](#approach-1-brute-force-rotation)
- [Approach 2: Using Extra Array](#approach-2-using-extra-array)
- [Approach 3: Cyclic Replacements](#approach-3-cyclic-replacements)
- [Approach 4: Reverse Approach](#approach-4-reverse-approach)

---

### Approach 1: Brute Force Rotation

In this approach, we repeatedly shift the array to the right by one position for `k` times. This is a straightforward method but not very efficient for large arrays.

**Intuition**:
- To rotate the array by one position to the right, move each element in the array one index to the right, and wrap the last element to the front.
- Repeat this shift `k` times.

```cpp
class Solution {
public:
    void rotate(vector<int>& nums, int k) {
        int n = nums.size();
        k = k % n;  // In case k is larger than n
        for (int i = 0; i < k; ++i) {
            int lastElement = nums[n - 1];
            // Shift all elements to the right
            for (int j = n - 1; j > 0; --j) {
                nums[j] = nums[j - 1];
            }
            nums[0] = lastElement;  // Place last element at the front
        }
    }
};
```

- **Time Complexity**: \(O(n \cdot k)\), where `n` is the number of elements in the array.
- **Space Complexity**: \(O(1)\), as it only uses a constant amount of extra space.

---

### Approach 2: Using Extra Array

Instead of rotating the array in place, we can use an additional array to store the rotated version of the array.

**Intuition**:
- Create a new array `temp`, where each element at index `i` in the original array is copied to index `(i + k) % n` in `temp`.
- Copy the new positions back into the original array.

```cpp
class Solution {
public:
    void rotate(vector<int>& nums, int k) {
        int n = nums.size();
        k = k % n;  // Handle cases where k is greater than n
        vector<int> temp(n);  // Temporary array to store rotated version
        
        for (int i = 0; i < n; ++i) {
            temp[(i + k) % n] = nums[i];  // Calculate new position for each element
        }
        
        // Copy rotated array back to original array
        for (int i = 0; i < n; ++i) {
            nums[i] = temp[i];
        }
    }
};
```

- **Time Complexity**: \(O(n)\), as it iterates through the array twice.
- **Space Complexity**: \(O(n)\), due to the extra array used.

---

### Approach 3: Cyclic Replacements

We can think of the problem as a cycle of positions that elements need to move through. This approach involves moving each element directly to its correct location using cycles.

**Intuition**:
- Calculate the new position for each element and move elements cyclically based on the GCD of `n` and `k` until the start point is reached again.

```cpp
class Solution {
public:
    void rotate(vector<int>& nums, int k) {
        int n = nums.size();
        k = k % n;
        int count = 0;
        
        for (int start = 0; count < n; ++start) {
            int current = start;
            int prev = nums[start];
            do {
                int next = (current + k) % n;
                int temp = nums[next];
                nums[next] = prev;
                prev = temp;
                current = next;
                ++count;  // Track number of elements placed
            } while (start != current);
        }
    }
};
```

- **Time Complexity**: \(O(n)\), since each element is moved once.
- **Space Complexity**: \(O(1)\), as it uses constant space aside from the input.

---

### Approach 4: Reverse Approach

The most optimal solution involves reversing segments of the array to achieve the same goal.

**Intuition**:
- Reverse the whole array.
- Reverse the first `k` elements.
- Reverse the remaining `n-k` elements.

```cpp
class Solution {
public:
    void rotate(vector<int>& nums, int k) {
        int n = nums.size();
        k = k % n;  // Adjust k if it's greater than n
        
        // Helper function to reverse a portion of the array
        auto reverse = [&](int start, int end) {
            while (start < end) {
                swap(nums[start], nums[end]);
                start++;
                end--;
            }
        };
        
        reverse(0, n - 1);  // Reverse the whole array
        reverse(0, k - 1);  // Reverse the first k elements
        reverse(k, n - 1);  // Reverse the rest
        
    }
};
```

- **Time Complexity**: \(O(n)\), as it processes the entire array in three separate linear passes.
- **Space Complexity**: \(O(1)\), since it performs the operations in-place.

