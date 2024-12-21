# [Leetcode 1044: Longest Duplicate Substring](https://leetcode.com/problems/longest-duplicate-substring/)

## Approaches
- [Brute Force Approach](#brute-force-approach)
- [Binary Search + Rabin-Karp Algorithm](#binary-search--rabin-karp-algorithm)

## Brute Force Approach

### Intuition
The brute force approach to solve this problem is to check every possible substring and see if it occurs more than once in the string. Start by generating all substrings of the given string `s` and store them in a hashmap to track frequencies. If a substring appears more than once, it is a candidate for the longest duplicate substring. This approach is straightforward but not efficient due to its high time complexity.

### Implementation

```go
func longestDupSubstring(s string) string {
    n := len(s)
    longest := ""

    // Nested loops to generate all possible substrings
    for i := 0; i < n; i++ {
        for j := i + 1; j <= n; j++ {
            currSubstring := s[i:j]
            // Checking if substring appears more than once
            if strings.Count(s, currSubstring) > 1 && len(currSubstring) > len(longest) {
                longest = currSubstring
            }
        }
    }
    return longest
}
```

### Time Complexity
- **Time Complexity**: O(N^3), where N is the length of the string. Generating all substrings takes O(N^2) and each count operation takes O(N).
- **Space Complexity**: O(1), the algorithm works in-place and only stores a few variables.

## Binary Search + Rabin-Karp Algorithm

### Intuition
To improve efficiency, we combine binary search with the Rabin-Karp rolling hash algorithm. The binary search helps in efficiently narrowing down the possible length of the longest duplicate substring. Rabin-Karp helps in efficiently checking if a substring of a specific length exists more than once.

- Binary Search: Use binary search to find the maximum length of a duplicate substring. Start with lengths ranging from 1 to N-1.
- Rabin-Karp: For each length, use a hash table to see if any substring of that length appears twice using a rolling hash to compute substring hashes efficiently.

### Implementation

```go
func longestDupSubstring(s string) string {
    mod := int(1e9 + 7) // A large prime for modulus
    base := 256         // Base for the rolling hash
    n := len(s)
    
    // Helper function to check for duplicates of a certain length
    findDuplicate := func(length int) string {
        hashMap := make(map[int][]int) // stores substrings' start indices by their hash
        hash := 0
        baseL := 1 // base^length % mod
        for i := 0; i < length; i++ {
            hash = (hash*base + int(s[i])) % mod
            baseL = (baseL * base) % mod
        }
        hashMap[hash] = []int{0} // initial substring

        for start := 1; start <= n-length; start++ {
            // Rolling hash: calculate new hash by sliding the window
            hash = (hash*base - int(s[start-1])*baseL + int(s[start+length-1])) % mod

            if hash < 0 {
                hash += mod // Ensure positive hash
            }
            
            // Check if this hash has been seen
            if indices, exists := hashMap[hash]; exists {
                for _, idx := range indices {
                    if s[start:start+length] == s[idx:idx+length] {
                        return s[start : start+length]
                    }
                }
            }
            hashMap[hash] = append(hashMap[hash], start)
        }
        return ""
    }
    
    left, right := 1, n-1
    var result string

    // Binary search to find the maximum duplicate substring
    for left <= right {
        mid := (left + right) / 2
        dup := findDuplicate(mid)
        if dup != "" {
            result = dup
            left = mid + 1
        } else {
            right = mid - 1
        }
    }
    return result
}
```

### Time Complexity
- **Time Complexity**: O(N log N), where N is the length of the string. The binary search is O(log N) and each Rabin-Karp check is O(N).
- **Space Complexity**: O(N), for storing the hash maps used in Rabin-Karp technique.

This approach efficiently narrows down the longest duplicate substring, utilizing a combination of binary search for length optimization and rabin-karp hashing for fast substring checks.

