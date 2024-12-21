## [Leetcode 307: Range Sum Query - Mutable](https://leetcode.com/problems/range-sum-query-mutable/)

### Approach:

- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Segment Tree](#approach-2-segment-tree)
- [Approach 3: Binary Indexed Tree (Fenwick Tree)](#approach-3-binary-indexed-tree-fenwick-tree)

### Approach 1: Brute Force

#### Intuition:
To implement a simple solution, whenever there is an update on an element, modify the element directly in the array. For range queries, simply sum the specified sub-array. This approach is straightforward but inefficient for large datasets because both updates and sum computations are not optimized.

#### Time Complexity:
- `update`: O(1)
- `sumRange`: O(n), where n is the number of elements in the range.

#### Space Complexity: 
- O(1), additional space apart from input storage.

```cpp
class NumArray {
    vector<int> nums;
public:
    NumArray(vector<int> &nums) {
        this->nums = nums;
    }
    
    void update(int index, int val) {
        // Directly update the element in the array
        nums[index] = val;
    }
    
    int sumRange(int left, int right) {
        int sum = 0;
        // Iterate over the range to calculate sum
        for (int i = left; i <= right; ++i) {
            sum += nums[i];
        }
        return sum;
    }
};
```

### Approach 2: Segment Tree

#### Intuition:
A segment tree is a binary tree used for storing intervals or segments. It allows processing of dynamic range queries such as sum or minimum. This method will efficiently handle both update and sumRange operations in logarithmic time.

#### Time Complexity:
- `update`: O(log n)
- `sumRange`: O(log n)

#### Space Complexity:
- O(n), where n is the number of elements.

```cpp
class NumArray {
    vector<int> segmentTree;
    int n;

    void buildTree(vector<int>& nums, int pos, int left, int right) {
        if (left == right) {
            segmentTree[pos] = nums[left];
            return;
        }
        int mid = (left + right) / 2;
        buildTree(nums, 2*pos+1, left, mid);
        buildTree(nums, 2*pos+2, mid+1, right);
        segmentTree[pos] = segmentTree[2*pos+1] + segmentTree[2*pos+2];
    }

    void updateTree(int pos, int left, int right, int index, int val) {
        if (left == right) {
            segmentTree[pos] = val;
            return;
        }
        int mid = (left + right) / 2;
        if (index <= mid) {
            updateTree(2*pos+1, left, mid, index, val);
        } else {
            updateTree(2*pos+2, mid+1, right, index, val);
        }
        segmentTree[pos] = segmentTree[2*pos+1] + segmentTree[2*pos+2];
    }

    int rangeSum(int pos, int left, int right, int qleft, int qright) {
        if (qleft <= left && qright >= right) {
            return segmentTree[pos];
        }
        if (qleft > right || qright < left) {
            return 0;
        }
        int mid = (left + right) / 2;
        return rangeSum(2*pos+1, left, mid, qleft, qright) + rangeSum(2*pos+2, mid+1, right, qleft, qright);
    }

public:
    NumArray(vector<int> &nums) {
        if (!nums.empty()) {
            n = nums.size();
            segmentTree.resize(4 * n, 0);
            buildTree(nums, 0, 0, n - 1);
        }
    }
    
    void update(int index, int val) {
        updateTree(0, 0, n - 1, index, val);
    }
    
    int sumRange(int left, int right) {
        return rangeSum(0, 0, n - 1, left, right);
    }
};
```

### Approach 3: Binary Indexed Tree (Fenwick Tree)

#### Intuition:
The BIT (or Fenwick Tree) is ideal for dynamic cumulative frequency tables and is used for efficiently executing point update operations and prefix sum queries. It can achieve a better trade-off with space by maintaining cumulative frequencies.

#### Time Complexity:
- `update`: O(log n)
- `sumRange`: O(log n)

#### Space Complexity:
- O(n), where n is the number of elements.

```cpp
class NumArray {
    vector<int> BIT;  
    vector<int> nums; 
    int n;

    void updateBIT(int index, int val) {
        while (index <= n) {
            BIT[index] += val;
            index += index & (-index);
        }
    }

    int queryBIT(int index) {
        int sum = 0;
        while (index > 0) {
            sum += BIT[index];
            index -= index & (-index);
        }
        return sum;
    }

public:
    NumArray(vector<int> &nums) {
        this->n = nums.size();
        this->nums = nums;
        BIT.resize(n + 1, 0);
        for (int i = 0; i < n; ++i) {
            updateBIT(i + 1, nums[i]);
        }
    }
    
    void update(int index, int val) {
        int delta = val - nums[index];
        nums[index] = val;
        updateBIT(index + 1, delta);
    }
    
    int sumRange(int left, int right) {
        return queryBIT(right + 1) - queryBIT(left);
    }
};
```

Each approach showcases different methodologies to tackle the problem while highlighting their respective time and space complexities. The Segment Tree and BIT improve upon the brute force method in terms of query performance, each with their unique implementation and update strategies.

