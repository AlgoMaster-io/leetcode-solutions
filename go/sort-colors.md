# [Leetcode 75: Sort Colors](https://leetcode.com/problems/sort-colors/)

## Approaches
1. [Counting Sort](#approach-1-counting-sort)
2. [One-pass with Dutch National Flag Algorithm](#approach-2-one-pass-with-dutch-national-flag-algorithm)

### Approach 1: Counting Sort

#### Intuition
This problem can be interpreted as a sorting problem where we need to sort a list of three different "colors" represented by numbers: 0, 1, and 2. A simple approach is to count the occurrences of each color, and then overwrite the original list with the sorted colors according to these counts.

#### Steps
1. Traverse the list once and count the occurrences of 0s, 1s, and 2s.
2. Overwrite the original list based on the counts. First, fill in the number of 0s, followed by the number of 1s, and finally the number of 2s.

**Code:**

```go
func sortColors(nums []int) {
    count := [3]int{} // Create an array to count occurrences of 0, 1, and 2

    // Count each number
    for _, num := range nums {
        count[num]++
    }

    // Overwrite nums with sorted elements based on counts
    index := 0
    for i := 0; i < 3; i++ {
        for j := 0; j < count[i]; j++ {
            nums[index] = i
            index++
        }
    }
}
```

**Complexity Analysis:**

- **Time Complexity**: O(n), where n is the length of the input array. We only traverse the array a few times.
- **Space Complexity**: O(1), since we only use a fixed amount of extra space.

### Approach 2: One-pass with Dutch National Flag Algorithm

#### Intuition
A more efficient way to solve this problem is by using the Dutch National Flag Algorithm, which works in a single pass. The idea is to maintain three pointers: `low`, `mid`, and `high` that help partition the array into three segments: 0s (`[0...low-1]`), 1s (`[low...mid-1]`), unprocessed (`[mid...high]`), and 2s (`[high+1...n-1]`).

#### Steps
1. Initialize `low` and `mid` to the beginning of the list and `high` to the end.
2. Iterate through the list using the `mid` pointer:
   - If the element at `mid` is 0, swap it with the element at `low` and increment both `low` and `mid`.
   - If the element at `mid` is 1, just move `mid` forward.
   - If the element at `mid` is 2, swap it with the element at `high` and decrement `high`.

**Code:**

```go
func sortColors(nums []int) {
    low, mid, high := 0, 0, len(nums)-1

    for mid <= high {
        switch nums[mid] {
        case 0:
            // Swap nums[mid] with nums[low], increment low and mid
            nums[low], nums[mid] = nums[mid], nums[low]
            low++
            mid++
        case 1:
            // 1 is in the right place, just move mid forward
            mid++
        case 2:
            // Swap nums[mid] with nums[high], decrement high
            nums[mid], nums[high] = nums[high], nums[mid]
            high--
        }
    }
}
```

**Complexity Analysis:**

- **Time Complexity**: O(n), where n is the length of the input list. Each element is processed at most once.
- **Space Complexity**: O(1), as we only use constant space for the pointers. 

The Dutch National Flag Algorithm is optimal for this problem as it sorts in-place with a single pass through the list.

