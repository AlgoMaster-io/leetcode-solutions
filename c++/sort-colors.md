# [Leetcode 75: Sort Colors](https://leetcode.com/problems/sort-colors/)

## Solutions:
- [Brute Force Approach (Two-Pass Counting Sort)](#brute-force-approach-two-pass-counting-sort)
- [Optimized Approach (One-Pass Dutch National Flag Problem)](#optimized-approach-one-pass-dutch-national-flag-problem)

### Brute Force Approach (Two-Pass Counting Sort)

#### Intuition:
The simplest way to sort colors in this set problem (where colors are represented as 0, 1, and 2) is by counting each occurrence of the numbers and then overwriting the original array in sorted order. This is a two-pass operation but effective given the constrained range of numbers.

#### Steps:
1. Traverse the array once and count the occurrences of 0s, 1s, and 2s.
2. Traverse the array again to overwrite it with the counted numbers.

#### Code:
```cpp
void sortColors(vector<int>& nums) {
    // Count the number of 0's, 1's, and 2's
    int count0 = 0, count1 = 0, count2 = 0;
    for (int num : nums) {
        if (num == 0) ++count0;
        else if (num == 1) ++count1;
        else if (num == 2) ++count2;
    }

    // Overwrite nums based on the counted values
    int i = 0;
    // Fill with 0s
    while (count0-- > 0) nums[i++] = 0;
    // Fill with 1s
    while (count1-- > 0) nums[i++] = 1;
    // Fill with 2s
    while (count2-- > 0) nums[i++] = 2;
}
```

#### Time Complexity:
- O(n), where n is the length of the array, since we pass through the array twice.
  
#### Space Complexity:
- O(1), no extra space is used beyond a few integer counters.

### Optimized Approach (One-Pass Dutch National Flag Problem)

#### Intuition:
A more efficient approach is inspired by the Dutch National Flag problem by Edsger Dijkstra. This method sorts the colors in a single pass using three pointers: one for the next 0 position, one for the next 2 position, and iterating through the array with a current index. By rearranging the indices of current, 0, and 2, we can sort the array in-place.

#### Steps:
1. Initialize three pointers: `low` at start, `mid` at start, and `high` at end.
2. Iterate with `mid`:
   - If `nums[mid]` is 0, swap it with `nums[low]` and increment both `low` and `mid`.
   - If `nums[mid]` is 1, just move the `mid` pointer.
   - If `nums[mid]` is 2, swap it with `nums[high]` and decrement `high`.

#### Code:
```cpp
void sortColors(vector<int>& nums) {
    int low = 0, mid = 0, high = nums.size() - 1;

    while (mid <= high) {
        if (nums[mid] == 0) {
            // Swap nums[mid] and nums[low]
            swap(nums[low], nums[mid]);
            ++low;
            ++mid;
        }
        else if (nums[mid] == 1) {
            // Just move mid
            ++mid;
        }
        else {
            // Swap nums[mid] and nums[high]
            swap(nums[mid], nums[high]);
            --high;
        }
    }
}
```

#### Time Complexity:
- O(n), as each element is considered at most twice in a single pass.

#### Space Complexity:
- O(1), all sorting is done in-place with constant extra space.

