# 315. [Count of Smaller Numbers After Self](https://leetcode.com/problems/count-of-smaller-numbers-after-self/)

## Approach 1: Brute Force

### Solution
```cpp
// Time Complexity: O(n^2)
// Space Complexity: O(n)
#include <vector>

class Solution {
public:
    std::vector<int> countSmaller(std::vector<int>& nums) {
        std::vector<int> result;

        // Traverse each element and count smaller numbers to its right
        for (int i = 0; i < nums.size(); i++) {
            int count = 0;
            for (int j = i + 1; j < nums.size(); j++) {
                if (nums[j] < nums[i]) {
                    count++;
                }
            }
            result.push_back(count);
        }

        return result;
    }
};
```

## Approach 2: Binary Indexed Tree (Fenwick Tree)

### Solution
```cpp
// Time Complexity: O(n log n)
// Space Complexity: O(n)
#include <vector>
#include <algorithm>

class Solution {
public:
    std::vector<int> countSmaller(std::vector<int>& nums) {
        int n = nums.size();
        std::vector<int> result(n);
        std::vector<int> sortedNums(nums.begin(), nums.end());

        // Sort and remove duplicates from nums
        std::sort(sortedNums.begin(), sortedNums.end());
        std::vector<int> fenwickTree(n + 1, 0);

        // Traverse nums from right to left
        for (int i = n - 1; i >= 0; i--) {
            int rank = std::lower_bound(sortedNums.begin(), sortedNums.end(), nums[i]) - sortedNums.begin() + 1;
            result[i] = query(rank - 1, fenwickTree);
            update(rank, 1, fenwickTree);
        }

        return result;
    }

private:
    // Update the Fenwick Tree
    void update(int index, int value, std::vector<int>& fenwickTree) {
        while (index < fenwickTree.size()) {
            fenwickTree[index] += value;
            index += index & -index;
        }
    }

    // Query the prefix sum in the Fenwick Tree
    int query(int index, const std::vector<int>& fenwickTree) {
        int sum = 0;
        while (index > 0) {
            sum += fenwickTree[index];
            index -= index & -index;
        }
        return sum;
    }
};
```

## Approach 3: Merge Sort

### Solution
```cpp
// Time Complexity: O(n log n)
// Space Complexity: O(n)
#include <vector>

class Solution {
private:
    std::vector<int> counts;

public:
    std::vector<int> countSmaller(std::vector<int>& nums) {
        int n = nums.size();
        counts.resize(n);
        std::vector<int> indices(n);

        // Initialize indices
        for (int i = 0; i < n; ++i) {
            indices[i] = i;
        }

        // Perform merge sort
        mergeSort(nums, indices, 0, n - 1);
        
        return counts;
    }

private:
    // Perform merge sort and count smaller elements
    void mergeSort(const std::vector<int>& nums, std::vector<int>& indices, int left, int right) {
        if (left >= right) return;

        int mid = left + (right - left) / 2;
        mergeSort(nums, indices, left, mid);
        mergeSort(nums, indices, mid + 1, right);

        merge(nums, indices, left, mid, right);
    }

    // Merge two sorted halves and update counts
    void merge(const std::vector<int>& nums, std::vector<int>& indices, int left, int mid, int right) {
        std::vector<int> tempIndices(right - left + 1);
        int i = left, j = mid + 1, k = 0, smallerCount = 0;

        // Merge the two halves
        while (i <= mid && j <= right) {
            if (nums[indices[i]] <= nums[indices[j]]) {
                counts[indices[i]] += smallerCount;
                tempIndices[k++] = indices[i++];
            } else {
                smallerCount++;
                tempIndices[k++] = indices[j++];
            }
        }

        // Copy remaining elements
        while (i <= mid) {
            counts[indices[i]] += smallerCount;
            tempIndices[k++] = indices[i++];
        }
        while (j <= right) {
            tempIndices[k++] = indices[j++];
        }

        // Copy merged result back to indices
        std::copy(tempIndices.begin(), tempIndices.end(), indices.begin() + left);
    }
};
```

