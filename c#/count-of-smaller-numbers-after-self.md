# [315. Count of Smaller Numbers After Self](https://leetcode.com/problems/count-of-smaller-numbers-after-self/)

## Approaches
1. [Brute Force Approach](#brute-force-approach)
2. [Binary Indexed Tree Approach](#binary-indexed-tree-approach)
3. [Merge Sort Approach](#merge-sort-approach)

### Brute Force Approach

#### Intuition
For each element in the array, count how many of the following elements are smaller than it. This approach is straightforward but inefficient for large arrays as it involves a nested loop to compare each element with every other element coming after it.

#### Implementation

```csharp
public class Solution {
    public IList<int> CountSmaller(int[] nums) {
        List<int> result = new List<int>();
        
        // Loop through each element of nums
        for (int i = 0; i < nums.Length; i++) {
            int count = 0;
            
            // Compare the current element with the elements after it
            for (int j = i + 1; j < nums.Length; j++) {
                if (nums[j] < nums[i]) {
                    count++;
                }
            }
            
            // Append the count to results
            result.Add(count);
        }
        
        return result;
    }
}
```

#### Complexity Analysis
- **Time Complexity**: O(n^2), where n is the number of elements in the `nums` array. This is due to the two nested loops.
- **Space Complexity**: O(1), as no additional space is used other than the result list.

---

### Binary Indexed Tree Approach

#### Intuition
A Binary Indexed Tree (BIT) or Fenwick Tree can be used to efficiently calculate the counts. It allows for efficient update and prefix sum calculation. The intuition is to maintain a frequency count of the elements seen so far as you process from right to left.

#### Implementation

```csharp
public class Solution {
    public IList<int> CountSmaller(int[] nums) {
        int offset = 10000; // Offset negative values
        int size = 2 * 10000 + 1; // Size to accommodate the offset

        // BIT of size 'size'
        int[] BIT = new int[size];

        IList<int> result = new List<int>();

        for (int i = nums.Length - 1; i >= 0; i--) {
            int rank = nums[i] + offset;
            result.Insert(0, GetSum(BIT, rank - 1));
            Update(BIT, rank, 1);
        }

        return result;
    }

    // Function to update the BIT
    private void Update(int[] BIT, int index, int value) {
        while (index < BIT.Length) {
            BIT[index] += value;
            index += (index & -index);
        }
    }

    // Function to get prefix sum from BIT
    private int GetSum(int[] BIT, int index) {
        int sum = 0;
        while (index > 0) {
            sum += BIT[index];
            index -= (index & -index);
        }
        return sum;
    }
}
```

#### Complexity Analysis
- **Time Complexity**: O(n log n), as both update and query operations in BIT take O(log n) time and there are n elements.
- **Space Complexity**: O(n), the space needed for the BIT array.

---

### Merge Sort Approach

#### Intuition
A modification of merge sort can be used here. During the merging process, count how many times an element from the right array is placed before elements from the left array. Such inversions give the count of smaller elements after each element.

#### Implementation

```csharp
public class Solution {
    public IList<int> CountSmaller(int[] nums) {
        int n = nums.Length;
        int[] result = new int[n];
        int[] indices = new int[n];
        for (int i = 0; i < n; i++) {
            indices[i] = i;
        }

        MergeSort(nums, indices, result, 0, n - 1);
        return result.ToList();
    }

    private void MergeSort(int[] nums, int[] indices, int[] result, int left, int right) {
        if (left >= right) return;
        
        int mid = (left + right) / 2;
        MergeSort(nums, indices, result, left, mid);
        MergeSort(nums, indices, result, mid + 1, right);
        Merge(nums, indices, result, left, mid, right);
    }

    private void Merge(int[] nums, int[] indices, int[] result, int left, int mid, int right) {
        int l = left, r = mid + 1, rightCount = 0;
        List<int> tempIndices = new List<int>();

        while (l <= mid && r <= right) {
            if (nums[indices[r]] < nums[indices[l]]) {
                tempIndices.Add(indices[r]);
                rightCount++;
                r++;
            } else {
                result[indices[l]] += rightCount;
                tempIndices.Add(indices[l]);
                l++;
            }
        }

        while (l <= mid) {
            result[indices[l]] += rightCount;
            tempIndices.Add(indices[l]);
            l++;
        }
        
        while (r <= right) {
            tempIndices.Add(indices[r]);
            r++;
        }

        for (int i = left; i <= right; i++) {
            indices[i] = tempIndices[i - left];
        }
    }
}
```

#### Complexity Analysis
- **Time Complexity**: O(n log n), where n is the number of elements in the `nums` array, as the modified merge sort runs in O(n log n).
- **Space Complexity**: O(n), due to the use of an auxiliary array for indices and the intermediate storage during the merge step.

