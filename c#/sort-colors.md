# [LeetCode Problem 75: Sort Colors](https://leetcode.com/problems/sort-colors/)

## Solutions

- [Solution 1: Brute Force Approach (Two-Pass Counting Sort)](#solution-1)
- [Solution 2: Single-Pass Dutch National Flag Algorithm](#solution-2)

### Solution 1: Brute Force Approach (Two-Pass Counting Sort)

**Intuition:**

The problem statement describes sorting an array that consists only of three unique integers: 0, 1, and 2 (representing colors). A straightforward approach to solve this is through a counting sort. This approach involves counting the number of occurrences of each integer and then rewriting the array using the count, which effectively sorts it.

**Steps:**
1. Traverse the array to count the occurrences of 0s, 1s, and 2s.
2. Rewrite the original array based on these counts.

**Code:**

```csharp
public void SortColors(int[] nums) {
    int count0 = 0, count1 = 0, count2 = 0;

    // Count the number of 0s, 1s, and 2s
    foreach (int num in nums) {
        if (num == 0) {
            count0++;
        } else if (num == 1) {
            count1++;
        } else {
            count2++;
        }
    }

    // Overwrite the array with the correct number of 0s, 1s, and 2s
    for (int i = 0; i < nums.Length; i++) {
        if (i < count0) {
            nums[i] = 0;
        } else if (i < count0 + count1) {
            nums[i] = 1;
        } else {
            nums[i] = 2;
        }
    }
}
```

**Time Complexity:** O(n), where n is the length of the array.  
**Space Complexity:** O(1), since we are not using any additional data structures that grow with input size.

---

### Solution 2: Single-Pass Dutch National Flag Algorithm

**Intuition:**

To further optimize the solution, we can solve this problem in a single pass (O(n) time complexity) using the Dutch National Flag algorithm. The idea is to maintain three pointers: `low`, `mid`, and `high`. By the end of the algorithm, `low` handles the 0s, `mid` handles the 1s, and `high` handles the 2s through optimal swaps.

**Steps:**
1. Initialize three pointers: `low`, `mid`, and `high`.
2. Use a while loop that processes elements until `mid` surpasses `high`.
3. Depending on the value at `mid`, perform swaps:
   - If nums[mid] is 0, swap it with nums[low], increment both `low` and `mid`.
   - If nums[mid] is 1, just increment `mid`.
   - If nums[mid] is 2, swap it with nums[high] and decrement `high`.

**Code:**

```csharp
public void SortColors(int[] nums) {
    int low = 0, mid = 0, high = nums.Length - 1;

    while (mid <= high) {
        if (nums[mid] == 0) {
            // Swap nums[low] with nums[mid]
            int temp = nums[low];
            nums[low] = nums[mid];
            nums[mid] = temp;
            low++;
            mid++;
        } else if (nums[mid] == 1) {
            // Move mid pointer forward
            mid++;
        } else if (nums[mid] == 2) {
            // Swap nums[high] with nums[mid]
            int temp = nums[mid];
            nums[mid] = nums[high];
            nums[high] = temp;
            high--;
        }
    }
}
```

**Time Complexity:** O(n), since each element is processed at most once.  
**Space Complexity:** O(1), as no additional arrays or data structures are used.

