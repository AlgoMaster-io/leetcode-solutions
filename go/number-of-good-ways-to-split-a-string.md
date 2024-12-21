[Leetcode 1525: Number of Good Ways to Split a String](https://leetcode.com/problems/number-of-good-ways-to-split-a-string/)

## Approaches
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Using Prefix and Suffix Arrays](#approach-2-using-prefix-and-suffix-arrays)

### Approach 1: Brute Force

#### Intuition
The brute force solution involves checking each possible split and counting the number of unique characters on each side of the split. For each index `i` in the string `s`, you calculate the number of distinct characters in `s[0:i]` and `s[i:]`. If they are equal, it's a valid split.

#### Code
```go
func numSplits(s string) int {
    n := len(s)
    if n < 2 {
        return 0
    }
    count := 0

    // Iterate over each possible split point
    for i := 1; i < n; i++ {
        leftUnique := make(map[byte]bool)
        rightUnique := make(map[byte]bool)
        
        // Count unique characters in left substring
        for j := 0; j < i; j++ {
            leftUnique[s[j]] = true
        }

        // Count unique characters in right substring
        for j := i; j < n; j++ {
            rightUnique[s[j]] = true
        }

        // If number of unique characters match, increment count
        if len(leftUnique) == len(rightUnique) {
            count++
        }
    }
    return count
}
```

#### Time Complexity
- **O(n^2)**: We have a nested loop, where for each split we iterate through the string to count unique characters.
  
#### Space Complexity
- **O(n)**: Two maps are used to count unique characters in the left and right part of the string.

### Approach 2: Using Prefix and Suffix Arrays

#### Intuition
To optimize the solution, we can precompute the number of unique characters at each split using prefix and suffix arrays. We keep track of how many distinct characters are in the prefix (left) part and suffix (right) part for each position. These arrays allow us to calculate the number of distinct characters in constant time for each split point.

#### Code
```go
func numSplits(s string) int {
    n := len(s)
    if n < 2 {
        return 0
    }
    
    leftUnique := make([]int, n)
    rightUnique := make([]int, n)
    seen := make(map[byte]bool)
    
    // Calculate unique characters for the prefix
    uniqueCount := 0
    for i := 0; i < n; i++ {
        if !seen[s[i]] {
            uniqueCount++
            seen[s[i]] = true
        }
        leftUnique[i] = uniqueCount
    }
    
    // Reset seen map and calculate unique characters for the suffix
    seen = make(map[byte]bool)
    uniqueCount = 0
    for i := n - 1; i >= 0; i-- {
        if !seen[s[i]] {
            uniqueCount++
            seen[s[i]] = true
        }
        rightUnique[i] = uniqueCount
    }
    
    // Count the number of good splits
    goodSplits := 0
    for i := 0; i < n-1; i++ {
        if leftUnique[i] == rightUnique[i+1] {
            goodSplits++
        }
    }
    
    return goodSplits
}
```

#### Time Complexity
- **O(n)**: We traverse the string three times to calculate prefix and suffix arrays and to count good splits.

#### Space Complexity
- **O(n)**: To store the prefix and suffix arrays which contain the number of unique characters at each index.

