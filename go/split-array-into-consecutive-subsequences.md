# [Leetcode 659: Split Array into Consecutive Subsequences](https://leetcode.com/problems/split-array-into-consecutive-subsequences/)

## Approaches
- [Approach 1: Greedy with Frequency and Subsequence Tracking](#approach-1-greedy-with-frequency-and-subsequence-tracking)

---

## Approach 1: Greedy with Frequency and Subsequence Tracking

### Intuition
The problem requires splitting the input array into consecutive subsequences of at least length 3. A greedy strategy is beneficial in this situation because it evaluates each element in context, attempting to extend existing subsequences or starting new ones when necessary.

To implement this approach:
1. Use a frequency map to count the occurrences of each number.
2. Use a hypothetical ending map to keep track of subsequences ending at each number.
3. Iterate through the array and for each number try to:
    - Extend an existing subsequence.
    - Start a new subsequence.
4. If neither extending nor starting a subsequence is possible, return `false`.

### Code

```go
func isPossible(nums []int) bool {
    // Map to store the frequency of each number
    freq := make(map[int]int)
    // Map to store how many subsequences end with each number
    ends := make(map[int]int)

    // Populate the frequency map
    for _, num := range nums {
        freq[num]++
    }

    // Iterate over the numbers to create or extend subsequences
    for _, num := range nums {
        if freq[num] == 0 {
            continue // If the number's frequency is 0, it cannot be used
        }

        // Attempt to extend a subsequence
        if ends[num-1] > 0 {
            ends[num-1]-- // Use one subsequence that ends with num-1
            ends[num]++   // Extend it to end with num
        } else if freq[num+1] > 0 && freq[num+2] > 0 {
            // Start a new subsequence that is at least 3 long
            freq[num+1]--
            freq[num+2]--
            ends[num+2]++
        } else {
            return false // Cannot use num in any subsequence, return false
        }
        freq[num]-- // Decrement the frequency as num is used
    }

    return true
}
```

### Time and Space Complexity
- **Time Complexity**: \(O(n)\), where \(n\) is the number of elements in the input array. Each element is processed at most once.
- **Space Complexity**: \(O(n)\) in the worst case for storing the frequency and ends maps, but averages better due to the constraints on input number range.

This approach efficiently solves the problem by prioritizing the extension of existing subsequences while keeping operations to a minimum leveraging hash maps for constant time checks and updates.

