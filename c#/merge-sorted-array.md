# [LeetCode 88: Merge Sorted Array](https://leetcode.com/problems/merge-sorted-array/)

## Approaches
1. [Brute Force using Sorting](#brute-force-using-sorting)
2. [Two Pointers from Start](#two-pointers-from-start)
3. [Two Pointers from End (Optimal)](#two-pointers-from-end-optimal)

---

### Brute Force using Sorting

#### Intuition:
The simplest approach to solve this problem is to coalesce both arrays into one and then sort it. This approach, although not optimal, is simple to implement and serves as a starting point for understanding the solution.

#### Steps:
1. Copy elements of `nums2` into `nums1` starting from index `m`.
2. Sort the combined array `nums1`.

#### Code:
```csharp
public void Merge(int[] nums1, int m, int[] nums2, int n) 
{
    // Copy elements from nums2 into nums1
    for (int i = 0; i < n; i++)
    {
        nums1[m + i] = nums2[i];
    }
    
    // Sort the merged array
    Array.Sort(nums1);
}
```

#### Complexity:
- **Time Complexity:** O((m + n) log(m + n)), due to sorting the array.
- **Space Complexity:** O(1), as we sort in-place.

---

### Two Pointers from Start

#### Intuition:
A more efficient approach involves using two pointers, one for each array. We'll create an auxiliary array where we'll store the merged result. This reduces the need to sort the entire array at the end.

#### Steps:
1. Initialize three pointers: `p1` at the start of `nums1`, `p2` at the start of `nums2`, and `p3` for the auxiliary array.
2. Compare elements at `p1` and `p2` and insert the smaller into the auxiliary array.
3. Move the pointer(s) accordingly.
4. Once one of the arrays is fully traversed, add remaining elements from the other array to the auxiliary array.
5. Copy the auxiliary array back to `nums1`.

#### Code:
```csharp
public void Merge(int[] nums1, int m, int[] nums2, int n) 
{
    int[] result = new int[m + n];
    int p1 = 0, p2 = 0, p3 = 0;
    
    // Traverse both arrays
    while (p1 < m && p2 < n)
    {
        if (nums1[p1] < nums2[p2])
        {
            result[p3++] = nums1[p1++];
        }
        else
        {
            result[p3++] = nums2[p2++];
        }
    }
    
    // If there are elements left in nums1
    while (p1 < m)
    {
        result[p3++] = nums1[p1++];
    }
    
    // If there are elements left in nums2
    while (p2 < n)
    {
        result[p3++] = nums2[p2++];
    }
    
    // Copy result back to nums1
    for (int i = 0; i < m + n; i++)
    {
        nums1[i] = result[i];
    }
}
```

#### Complexity:
- **Time Complexity:** O(m + n), as we traverse each element once.
- **Space Complexity:** O(m + n), for the auxiliary array.

---

### Two Pointers from End (Optimal)

#### Intuition:
To optimize space further, we can eliminate the auxiliary array by starting the merge from the end of `nums1`. We take advantage of the fact that `nums1` has enough space to accommodate all elements.

#### Steps:
1. Set pointer `p1` at `m - 1` for `nums1`, and `p2` at `n - 1` for `nums2`.
2. Set `p` at `m + n - 1` for placing elements in `nums1`.
3. Compare elements at `p1` and `p2` and place the larger at position `p`.
4. Decrement the relevant pointers.
5. Continue until all elements of `nums2` are placed in `nums1`.

#### Code:
```csharp
public void Merge(int[] nums1, int m, int[] nums2, int n) 
{
    int p1 = m - 1, p2 = n - 1, p = m + n - 1;
    
    // Start merging from the end
    while (p2 >= 0)
    {
        if (p1 >= 0 && nums1[p1] > nums2[p2])
        {
            nums1[p--] = nums1[p1--];
        }
        else
        {
            nums1[p--] = nums2[p2--];
        }
    }
}
```

#### Complexity:
- **Time Complexity:** O(m + n), as we process each element once.
- **Space Complexity:** O(1), as we perform in-place merging without extra space.

