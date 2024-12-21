# [Leetcode 523: Continuous Subarray Sum](https://leetcode.com/problems/continuous-subarray-sum/)

## Approaches:
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Prefix Sums and HashMap](#approach-2-prefix-sums-and-hashmap)


## Approach 1: Brute Force

### Intuition:
The simplest way to solve the problem is to check all possible subarrays and see if any of them sum up to a multiple of `k`. We do this by iterating over all subarrays, calculating their sums, and checking the condition.

### Steps:
1. Iterate through the array using two nested loops; `i` for the start of the subarray and `j` for the end.
2. Calculate the sum of elements from the start index `i` to `j`.
3. For each subarray (from `i` to `j`), check if the sum is a multiple of `k` and if the subarray length is at least 2.
4. If such a subarray is found, return `true`.
5. Otherwise, after checking all subarrays, return `false`.

### Code:
```go
func checkSubarraySum(nums []int, k int) bool {
    n := len(nums)
    for i := 0; i < n; i++ {
        sum := 0
        for j := i; j < n; j++ {
            sum += nums[j]
            // Check if sum is a multiple of k and subarray has at least 2 elements
            if j-i+1 >= 2 && sum%k == 0 {
                return true
            }
        }
    }
    return false
}
```

### Time Complexity:
- O(n^2), where n is the length of the array. We are checking all possible subarrays.
### Space Complexity:
- O(1), as we are not using any extra space besides a few integer variables.

## Approach 2: Prefix Sums and HashMap

### Intuition:
Instead of recalculating the sum for every subarray, we can maintain a running cumulative sum while iterating over the array. If at any point, the cumulative sum modulo `k` has been seen before (in a previous index), it means the subarray between those points sums up to a multiple of `k`.

### Steps:
1. Use a map to store the first occurrence of each modulo result of the cumulative sum by `k`.
2. Initialize the map with a base case of modulo result `0` at index `-1` to cater for the whole subarray starting from index `0`.
3. Iterate over the array, updating the cumulative sum.
4. Calculate `sum % k` for the current cumulative sum.
5. If this modulo result is already in the map and the distance between the current index and stored index is at least 2, return `true`.
6. If not, store the current index for this modulo result if it has not been stored before.
7. Return `false` if no valid subarray is found.

### Code:
```go
func checkSubarraySum(nums []int, k int) bool {
    modMap := make(map[int]int)
    modMap[0] = -1  // Base case to handle subarrays starting at index 0
    sum := 0

    for i, num := range nums {
        sum += num
        mod := sum % k

        // Adjust mod to handle negative sums
        if mod < 0 {
            mod += k
        }

        if prevIndex, exists := modMap[mod]; exists {
            if i-prevIndex >= 2 {
                return true
            }
        } else {
            modMap[mod] = i
        }
    }
    return false
}
```

### Time Complexity:
- O(n), where n is the length of the array. We iterate over the array once while utilizing a map to store results.
### Space Complexity:
- O(min(n, k)), as we store at most n different modulo results in the map.

