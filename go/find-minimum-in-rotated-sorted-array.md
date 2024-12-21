# [Leetcode 153: Find Minimum in Rotated Sorted Array](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/)

## Approaches:
1. [Linear Search](#linear-search)
2. [Binary Search](#binary-search)

---

### 1. Linear Search

#### Intuition:
In a rotated sorted array, the elements are first sorted in increasing order, and then an arbitrary number of elements from the beginning of the array are moved to the end. The minimum element in such an array would be the point of deviation from the sorted order. Therefore, a simple way to find the minimum is by traversing through the array and finding the element that is smaller than its successor. If we don't find such an element, it means the array is sorted entirely and not rotated at all.

#### Go Code:
```go
func findMinLinear(nums []int) int {
    // Initialize the minimum as the first element
    min := nums[0]
    for i := 1; i < len(nums); i++ {
        // If the next element is smaller, update the minimum
        if nums[i] < min {
            min = nums[i]
        }
    }
    return min
}
```

#### Time Complexity:
- O(n), where n is the number of elements in the array. In the worst case, we may need to check all elements of the array.

#### Space Complexity:
- O(1), no extra space is used apart from a few variables.

---

### 2. Binary Search

#### Intuition:
When an array is initially sorted and then rotated, the array can be divided into two sorted subarrays. Our task is to find the pivot point, which is the smallest element. We can efficiently find this by using a modified binary search. The critical observation is if the subarray between the left and mid indices is sorted, the minimum value lies in the other half, otherwise it's in the current half. Repeat narrowing down the interval using binary search until the interval contains only one element, which will be the smallest.

#### Go Code:
```go
func findMinBinary(nums []int) int {
    left, right := 0, len(nums) - 1
    for left < right {
        mid := left + (right - left) / 2
        // If middle element is greater than the rightmost element
        // The minimum is in the right half
        if nums[mid] > nums[right] {
            left = mid + 1
        } else { // Else the minimum is in the left half (including mid)
            right = mid
        }
    }
    // After the loop, left == right, which points to the smallest element
    return nums[left]
}
```

#### Time Complexity:
- O(log n), where n is the number of elements in the array. Each binary search step halves the search interval.

#### Space Complexity:
- O(1), constant space is used.

---

Both approaches solve the problem, but the Binary Search approach is more efficient for large arrays as it significantly reduces the time complexity from linear to logarithmic.

