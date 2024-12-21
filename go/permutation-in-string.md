# [Leetcode 567: Permutation in String](https://leetcode.com/problems/permutation-in-string/)

## Approaches
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Sliding Window with Count Arrays](#approach-2-sliding-window-with-count-arrays)
- [Approach 3: Sliding Window with HashMaps (Optimized)](#approach-3-sliding-window-with-hashmaps-optimized)

---

## Approach 1: Brute Force

**Intuition**

The brute force approach would be to generate all possible permutations of the first string and then check if any of these permutations is a substring of the second string. While this guarantees correctness, it is highly inefficient because generating permutations has a high computational cost.

**Steps**
1. Generate all permutations of string `s1`.
2. For each permutation, check if it exists as a substring within `s2`.
3. Return `true` if any permutation matches; otherwise, return `false`.

**Time and Space Complexity**

- **Time Complexity**: O(n! * m), where `n` is the length of `s1` and `m` is the length of `s2`. This arises because there are `n!` permutations.
- **Space Complexity**: O(n) for storing permutations.

**Go Code**

```go
// This approach is not implemented in Go due to its inefficiency and lack of direct permutation utilities.
```

---

## Approach 2: Sliding Window with Count Arrays

**Intuition**

Instead of generating permutations, we can utilize a sliding window of length equal to `s1` on `s2`. For each window position, we check if the frequency of characters in the current window matches that in `s1`. This is achieved using frequency arrays of size 26 (for each alphabet character).

**Steps**
1. Initialize character count arrays for `s1` and first window in `s2`.
2. Move the window one character at a time, updating the count arrays.
3. Compare the counts of characters in the window with `s1`'s character counts.
4. Return `true` if any window has matching counts.

**Time and Space Complexity**

- **Time Complexity**: O(n + m), where `n` is the length of `s1` and `m` is the length of `s2`.
- **Space Complexity**: O(1), because the size of the frequency array is fixed (26 characters).

**Go Code**

```go
func checkInclusion(s1 string, s2 string) bool {
    if len(s1) > len(s2) {
        return false
    }
    
    // Frequency arrays for s1 and the first window of s2
    s1Count := [26]int{}
    s2Count := [26]int{}
    
    for i := 0; i < len(s1); i++ {
        s1Count[s1[i]-'a']++
        s2Count[s2[i]-'a']++
    }
    
    // Helper function to compare frequency arrays
    matches := func() bool {
        for i := 0; i < 26; i++ {
            if s1Count[i] != s2Count[i] {
                return false
            }
        }
        return true
    }
    
    for i := len(s1); i < len(s2); i++ {
        if matches() {
            return true
        }
        // Slide the window: Add new character & Remove old character
        s2Count[s2[i] - 'a']++
        s2Count[s2[i-len(s1)] - 'a']--
    }
    
    return matches()
}
```

---

## Approach 3: Sliding Window with HashMaps (Optimized)

**Intuition**

This approach is a further optimization using two hash maps to track character counts, allowing for constant time comparisons as opposed to checking all character counts every iteration. By maintaining a `match` count, we can quickly decide if all characters align.

**Steps**
1. Use hash maps (or frequency arrays) to track character frequency.
2. As you move the window, increase or decrease the match count when characters enter or leave the window.
3. If the match count equals the number of characters without zero frequency in `s1`, return `true`.

**Time and Space Complexity**

- **Time Complexity**: O(n + m), with a more efficient window shifting operation than direct frequency comparison.
- **Space Complexity**: O(1), fixed-size data structure for character counts.

**Go Code**

```go
func checkInclusion(s1 string, s2 string) bool {
    if len(s1) > len(s2) {
        return false
    }
    
    s1Count := make(map[byte]int)
    s2Count := make(map[byte]int)
    
    for i := 0; i < len(s1); i++ {
        s1Count[s1[i]]++
        s2Count[s2[i]]++
    }
    
    matches := 0
    for char, count := range s1Count {
        if s2Count[char] == count {
            matches++
        }
    }
    
    l := 0
    for r := len(s1); r < len(s2); r++ {
        if matches == len(s1Count) {
            return true
        }
        
        currentChar := s2[r]
        s2Count[currentChar]++
        if count, exists := s1Count[currentChar]; exists {
            if s2Count[currentChar] == count {
                matches++
            } else if s2Count[currentChar] == count+1 {
                matches--
            }
        }
        
        leftChar := s2[l]
        s2Count[leftChar]--
        l++
        if count, exists := s1Count[leftChar]; exists {
            if s2Count[leftChar] == count {
                matches++
            } else if s2Count[leftChar] == count-1 {
                matches--
            }
        }
    }
    
    return matches == len(s1Count)
}
```

This optimized sliding window approach checks for permutations in a linear time with very minimal overhead, making it suitable for larger inputs.

