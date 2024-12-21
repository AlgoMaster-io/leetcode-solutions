# [Subarray Sums Divisible by K](https://leetcode.com/problems/subarray-sums-divisible-by-k/)

## Approaches
1. [Brute Force Approach](#brute-force-approach)
2. [Prefix Sum with HashMap Approach](#prefix-sum-with-hashmap-approach)

## Brute Force Approach

### Intuition:
A straightforward way to approach this problem is to consider every possible subarray and calculate its sum. We then check if this sum is divisible by `K`. While this is simple to understand and implement, it is inefficient because it requires examining each subarray individually.

### Solution:

```go
func subarraysDivByK(nums []int, K int) int {
    count := 0
    for start := 0; start < len(nums); start++ {
        sum := 0
        for end := start; end < len(nums); end++ {
            sum += nums[end] // calculate subarray sum from start to end
            if sum % K == 0 { // check if sum is divisible by K
                count++
            }
        }
    }
    return count
}
```

### Time Complexity:
- O(n^2), where `n` is the length of `nums`. We calculate the sum for all subarrays.
  
### Space Complexity:
- O(1), no extra space is used beyond input storage.

## Prefix Sum with HashMap Approach

### Intuition:
The key idea is to use the properties of the prefix sum and modulo operation. A prefix sum allows maintaining a cumulative sum of elements from the start of the array to a given index. With a hashmap, we can track how often each remainder has appeared. If the same remainder appears again, it forms a subarray whose sum is divisible by `K`.

### Solution:

```go
func subarraysDivByK(nums []int, K int) int {
    count := 0
    sum := 0
    remainderCount := make(map[int]int)
    remainderCount[0] = 1 // To handle the case where the sum itself is divisible by K

    for _, num := range nums {
        sum += num
        remainder := sum % K

        // Handling negative remainders
        if remainder < 0 {
            remainder += K
        }

        if val, found := remainderCount[remainder]; found {
            count += val
        }
        remainderCount[remainder]++ // increment the count of this remainder
    }
    return count
}
```

### Time Complexity:
- O(n), where `n` is the length of `nums`. We iterate through the array once.

### Space Complexity:
- O(K), for storing the count of remainders in the hashmap.


This solution is optimal because it combines the efficient calculation of prefix sums with the fast lookup properties of a hashmap, allowing us to effectively count subarrays without redundancy.

