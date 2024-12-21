[Leetcode 560: Subarray Sum Equals K](https://leetcode.com/problems/subarray-sum-equals-k/)

### Approaches:
1. [Brute Force Solution](#brute-force-solution)
2. [Cumulative Sum with Hash Map](#cumulative-sum-with-hash-map)

---

### Brute Force Solution

The brute force approach involves checking every possible subarray in the input array and calculating its sum. If the sum equals the target `k`, we increment our result counter. This straightforward solution works well for small input sizes but is inefficient for larger arrays.

#### Intuition:
- Iterate over all possible starting indices of subarrays.
- For each starting index, iterate over all possible ending indices, accumulating the sum.
- If the sum reaches `k`, increment the count.

#### Code:
```go
func subarraySum(nums []int, k int) int {
    count := 0
    n := len(nums)
    
    // Iterate over every possible starting index
    for start := 0; start < n; start++ {
        sum := 0
        // Iterate over every possible ending index, computing the sum of the subarray
        for end := start; end < n; end++ {
            sum += nums[end]
            // If sum matches k, increment the count
            if sum == k {
                count++
            }
        }
    }
    
    return count
}
```

#### Complexity:
- **Time Complexity**: O(n^2) - Two nested loops result in a quadratic time complexity.
- **Space Complexity**: O(1) - No additional space other than variables.

---

### Cumulative Sum with Hash Map

A more optimal solution uses a hash map to store previously seen cumulative sums. This allows us to reduce the problem to a lookup problem and permits a significant speedup from our brute force approach. By using a hash map, we can keep track of how many times each cumulative sum has occurred, allowing us to efficiently calculate when a subarray sum equals `k`.

#### Intuition:
- As we iterate through the array, calculate the cumulative sum up to each index.
- Check if `(cumulative sum - k)` exists in our hash map.
- If it does, add the frequency of that sum to the result since it indicates there are one or more subarrays that sum to `k`.

#### Code:
```go
func subarraySum(nums []int, k int) int {
    count := 0
    sum := 0
    sumMap := make(map[int]int)
    sumMap[0] = 1 // Initialize with zero sum for subarrays that start from index 0

    for _, num := range nums {
        sum += num
        // Check if there has been a sum that, when subtracted from the current sum, equals k
        if _, found := sumMap[sum-k]; found {
            count += sumMap[sum-k]
        }
        // Add the current cumulative sum to the map
        sumMap[sum]++
    }
    
    return count
}
```

#### Complexity:
- **Time Complexity**: O(n) - Single pass through the array with constant time lookups and updates to the hash map.
- **Space Complexity**: O(n) - In the worst case, the hash map can store `n` different sums.

