# [Leetcode 315: Count of Smaller Numbers After Self](https://leetcode.com/problems/count-of-smaller-numbers-after-self/)

## Approaches
1. [Brute Force Approach](#brute-force-approach)
2. [Binary Indexed Tree (Fenwick Tree) Approach](#binary-indexed-tree-fenwick-tree-approach)
3. [Merge Sort with Index Tracking Approach](#merge-sort-with-index-tracking-approach)

---

### Brute Force Approach

The first, basic approach is to use a simple brute force method. For every element in the list, the count of smaller numbers is calculated by traversing the rest of the list.

#### Intuition:
For each element, count how many of the subsequent numbers are smaller. This is the most straightforward approach and works well for small inputs, but is inefficient as input size grows due to its time complexity.

#### Code:
```cpp
class Solution {
public:
    vector<int> countSmaller(vector<int>& nums) {
        int n = nums.size();
        vector<int> result(n, 0);
        
        // Loop over each element in the array
        for (int i = 0; i < n; ++i) {
            // For the current element nums[i], count how many numbers following it are smaller
            for (int j = i + 1; j < n; ++j) {
                if (nums[j] < nums[i]) {
                    result[i]++;
                }
            }
        }
        return result;
    }
};
```

#### Complexity:
- **Time Complexity:** O(n^2), where n is the number of elements in the array. This is due to the nested loops.
- **Space Complexity:** O(1), if we don't consider the output list.

---

### Binary Indexed Tree (Fenwick Tree) Approach

For an optimized solution, we can use a Binary Indexed Tree (Fenwick Tree), which allows us to efficiently update frequencies and compute prefix sums.

#### Intuition:
A Binary Indexed Tree is used for fast update and prefix sum calculation allowing us to maintain and query the number of smaller elements efficiently. We process the elements from right to left, maintain a frequency of elements seen so far, and query the number of elements smaller than the current one before updating.

#### Code:
```cpp
class Solution {
public:
    vector<int> countSmaller(vector<int>& nums) {
        int offset = 10000;  // Offset for negative numbers
        int size = 2 * offset + 1;  // Size based on constraints
        vector<int> BIT(size + 1, 0);  // Binary Indexed Tree
        vector<int> result(nums.size(), 0);
        
        // Converts index based on offset, handles negatives
        auto update = [&](int index, int value) {
            while (index < BIT.size()) {
                BIT[index] += value;
                index += (index & -index);
            }
        };
        
        auto query = [&](int index) {
            int sum = 0;
            while (index > 0) {
                sum += BIT[index];
                index -= (index & -index);
            }
            return sum;
        };
        
        for (int i = nums.size() - 1; i >= 0; --i) {
            int num = nums[i] + offset;
            result[i] = query(num);
            update(num + 1, 1);
        }
        return result;
    }
};
```

#### Complexity:
- **Time Complexity:** O(n log m), where n is the number of elements and m is the range of input values after normalization.
- **Space Complexity:** O(m), where m is the range for maintaining the BIT array.

---

### Merge Sort with Index Tracking Approach

This is considered a highly efficient approach which combines sorting and counting in one go by modifying merge sort to count while sorting.

#### Intuition:
By using a modified merge sort approach, we not only sort the array but also as we merge, we can count how many elements on the right are smaller than those on the left.

#### Code:
```cpp
class Solution {
public:
    vector<int> countSmaller(vector<int>& nums) {
        int n = nums.size();
        vector<int> result(n, 0), indices(n, 0);
        iota(indices.begin(), indices.end(), 0); // Initialize indices with {0, 1, ..., n-1}
        
        // Perform a modified merge sort
        mergeSort(nums, indices, result, 0, n-1);
        return result;
    }

private:
    void mergeSort(vector<int>& nums, vector<int>& indices, vector<int>& result, int left, int right) {
        if (left >= right) return;
        
        int mid = left + (right - left) / 2;
        mergeSort(nums, indices, result, left, mid);
        mergeSort(nums, indices, result, mid + 1, right);
        merge(nums, indices, result, left, mid, right);
    }
    
    void merge(vector<int>& nums, vector<int>& indices, vector<int>& result, int left, int mid, int right) {
        vector<int> tempIndices(right - left + 1);
        int i = left, j = mid + 1, k = 0, rightCount = 0;
        
        while (i <= mid && j <= right) {
            if (nums[indices[j]] < nums[indices[i]]) {
                rightCount++;
                tempIndices[k++] = indices[j++];
            } else {
                result[indices[i]] += rightCount;
                tempIndices[k++] = indices[i++];
            }
        }
        
        while (i <= mid) {
            result[indices[i]] += rightCount;
            tempIndices[k++] = indices[i++];
        }
        
        while (j <= right) {
            tempIndices[k++] = indices[j++];
        }
        
        for (int i = left; i <= right; ++i) {
            indices[i] = tempIndices[i - left];
        }
    }
};
```

#### Complexity:
- **Time Complexity:** O(n log n), where n is the number of elements in the array. This is due to the merge sort algorithm.
- **Space Complexity:** O(n), necessary for storing temporary indices during merge.

