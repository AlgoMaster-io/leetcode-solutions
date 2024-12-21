# [Leetcode 493: Reverse Pairs](https://leetcode.com/problems/reverse-pairs/)

## Approaches
1. [Brute Force](#approach-1-brute-force)
2. [Merge Sort](#approach-2-merge-sort)
3. [Binary Indexed Tree (BIT) / Fenwick Tree](#approach-3-bit-fenwick-tree)

---

## Approach 1: Brute Force

### Intuition:

The brute force approach is straightforward, involving checking every possible pair of indices to see if they satisfy the condition: `nums[i] > 2*nums[j]` for `i < j`.

### Code:

```cpp
class Solution {
public:
    int reversePairs(vector<int>& nums) {
        int count = 0;
        int n = nums.size();
        for (int i = 0; i < n; ++i) {
            for (int j = i + 1; j < n; ++j) {
                // Check whether nums[i] > 2 * nums[j]
                if (nums[i] > 2LL * nums[j]) {
                    ++count;
                }
            }
        }
        return count;
    }
};
```

### Complexity Analysis:

- **Time Complexity**: O(n^2), where n is the number of elements in the array. This is because we are checking every possible pair, resulting in a quadratic number of comparisons.
- **Space Complexity**: O(1), we are not using any extra space except for temporary variables.

---

## Approach 2: Merge Sort

### Intuition:

Using a modified merge sort, we can count the significant reverse pairs during the merge process. The main idea is to sort the array and count the pairs satisfying the condition: `nums[i] > 2*nums[j]`. This takes advantage of the sorted order to skip unnecessary comparisons and thus improve efficiency.

### Code:

```cpp
class Solution {
public:
    int mergeAndCount(vector<int> &nums, int left, int mid, int right) {
        int count = 0;
        int j = mid + 1;
        
        // Count the significant pairs
        for (int i = left; i <= mid; ++i) {
            while (j <= right && nums[i] > 2LL * nums[j]) {
                j++;
            }
            count += (j - (mid + 1));
        }
        
        // Merge step
        vector<int> temp;
        int i = left, k = mid + 1;
        while (i <= mid && k <= right) {
            if (nums[i] <= nums[k]) {
                temp.push_back(nums[i++]);
            } else {
                temp.push_back(nums[k++]);
            }
        }
        while (i <= mid) {
            temp.push_back(nums[i++]);
        }
        while (k <= right) {
            temp.push_back(nums[k++]);
        }
        
        // Copy back to the original array
        for (int i = left; i <= right; ++i) {
            nums[i] = temp[i - left];
        }
        
        return count;
    }
    
    int mergeSortAndCount(vector<int> &nums, int left, int right) {
        if (left >= right) return 0;
        int mid = left + (right - left) / 2;
        int count = mergeSortAndCount(nums, left, mid);
        count += mergeSortAndCount(nums, mid + 1, right);
        count += mergeAndCount(nums, left, mid, right);
        return count;
    }
    
    int reversePairs(vector<int>& nums) {
        return mergeSortAndCount(nums, 0, nums.size() - 1);
    }
};
```

### Complexity Analysis:

- **Time Complexity**: O(n log n), because each merge step takes linear time and there are logarithmic merge steps.
- **Space Complexity**: O(n), due to the temporary arrays used in the merge process.

---

## Approach 3: Binary Indexed Tree (BIT) / Fenwick Tree

### Intuition:

This approach uses a Fenwick Tree (or Binary Indexed Tree) to efficiently count the reverse pairs. Fenwick Tree allows dynamic prefix sum updates and queries in logarithmic time. We first map the numbers to a smaller range to fit within the tree.

### Code:

```cpp
class Solution {
public:
    int reversePairs(vector<int>& nums) {
        set<long long> allNumbers;
        for (const int& num : nums) {
            allNumbers.insert(num);
            allNumbers.insert(2LL * num);
        }
        
        unordered_map<long long, int> ranks;
        int rank = 0;
        for (const long long& num : allNumbers) {
            ranks[num] = ++rank;
        }
        
        vector<int> fenwickTree(ranks.size() + 1, 0);
        
        auto update = [&](int i) {
            while (i < fenwickTree.size()) {
                fenwickTree[i]++;
                i += i & -i;
            }
        };
        
        auto query = [&](int i) {
            int sum = 0;
            while (i > 0) {
                sum += fenwickTree[i];
                i -= i & -i;
            }
            return sum;
        };
        
        int count = 0;
        // Check pairs in reverse order
        for (int i = nums.size() - 1; i >= 0; --i) {
            count += query(ranks[nums[i]] - 1);
            update(ranks[2LL * nums[i]]);
        }
        
        return count;
    }
};
```

### Complexity Analysis:

- **Time Complexity**: O(n log n), where n is the number of elements in the array. The insertion and query operations on the Fenwick Tree are logarithmic.
- **Space Complexity**: O(n), primarily due to the space required by the Fenwick Tree and the map for ranks.

