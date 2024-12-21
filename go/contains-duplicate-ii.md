# [Leetcode 219: Contains Duplicate II](https://leetcode.com/problems/contains-duplicate-ii/)

## Approaches
1. [Brute Force Approach](#brute-force-approach)
2. [HashMap Approach](#hashmap-approach)
3. [Sliding Window HashMap Approach](#sliding-window-hashmap-approach)

### Brute Force Approach

**Intuition**:  
In this approach, we will check each pair of indices `(i, j)` and find if there are any duplicate elements where the indices difference is less than or equal to `k`. This involves using two nested loops, where the outer loop picks the first element and the inner loop checks for duplicates in the subsequent elements within the given distance `k`.

**Algorithm**:
1. Iterate over each element `i` of the array.
2. For each element `i`, iterate through every element `j` from `i+1` to `i+k`.
3. If `nums[i]` equals `nums[j]`, return `true`.
4. If no such pair is found, return `false`.

**Complexity Analysis**:
- **Time Complexity**: O(n * k), where `n` is the length of the array. For each element, we have potentially `k` elements to check.
- **Space Complexity**: O(1), no extra space is used apart from a few variables.

```go
func containsNearbyDuplicate(nums []int, k int) bool {
    for i := 0; i < len(nums); i++ {
        for j := i + 1; j <= i+k && j < len(nums); j++ {
            // Check if the elements at i and j are the same
            if nums[i] == nums[j] {
                return true
            }
        }
    }
    return false
}
```

### HashMap Approach

**Intuition**:  
Instead of checking each pair, use a HashMap to store the most recent index of each number. As we iterate through the array, we can quickly check if an element has appeared within the previous `k` indices.

**Algorithm**:
1. Initialize a map to store the indices of elements.
2. Iterate through the array:
   - If the element exists in the map and the difference between the current index and the stored index is less than or equal to `k`, return `true`.
   - Otherwise, update the map with the current index of the element.
3. If no duplicate is found within the required range, return `false`.

**Complexity Analysis**:
- **Time Complexity**: O(n), where `n` is the length of the array, as we only make a single pass through the array.
- **Space Complexity**: O(n), since in the worst case, we might store all elements in the map.

```go
func containsNearbyDuplicate(nums []int, k int) bool {
    indexMap := make(map[int]int)
    for i, num := range nums {
        if idx, exists := indexMap[num]; exists && i-idx <= k {
            // If a duplicate is found within the distance k, return true
            return true
        }
        // Update the map with the current index of the element
        indexMap[num] = i
    }
    return false
}
```

### Sliding Window HashMap Approach

**Intuition**:  
Further optimization can be done using a "Sliding Window" approach with the HashMap. As we only need to keep track of elements within the current window of size `k`.

**Algorithm**:
1. For simplicity, use a map to track elements within the current window.
2. Iterate through the array:
   - If the current index `i` is greater than `k`, remove the element that is no longer in the window (i.e., `nums[i-k-1]`).
   - Check if the current element already exists in the map.
   - Add/Update the current element to the map.
   - If a duplicate is found at any point, return `true`.
3. If no duplicate is found within any window, return `false`.

**Complexity Analysis**:
- **Time Complexity**: O(n), as we're iterating through the array once.
- **Space Complexity**: O(min(n, k)), since we maintain a map at most of size `k`.

```go
func containsNearbyDuplicate(nums []int, k int) bool {
    windowMap := make(map[int]struct{})
    for i, num := range nums {
        if i > k {
            // Slide the window to remove the oldest element
            delete(windowMap, nums[i-k-1])
        }
        if _, exists := windowMap[num]; exists {
            // Duplicate within the window found
            return true
        }
        // Add the current element to the map
        windowMap[num] = struct{}{}
    }
    return false
}
```

Each approach builds upon the intuitions of efficiently managing checked elements and their indices, considering different trade-offs between simplicity and performance.

