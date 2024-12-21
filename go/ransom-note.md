## [LeetCode Problem 383: Ransom Note](https://leetcode.com/problems/ransom-note/)

### Solutions:
1. [Basic Approach: Brute Force](#basic-approach-brute-force)
2. [Intermediate Approach: Frequency Count](#intermediate-approach-frequency-count)
3. [Optimal Approach: Using Go's Map](#optimal-approach-using-gos-map)

---

### Basic Approach: Brute Force

**Intuition:**

The brute force solution would involve checking each character of the `ransomNote` in the magazine. If it's found, we remove it from the magazine to prevent repeated use. This involves a fair amount of string manipulation, making it inefficient.

**Steps:**

1. Convert `magazine` to a slice of runes to easily handle character removal.
2. For each character in the `ransomNote`, iterate through `magazine` to find it.
3. If found, remove the character from `magazine`.
4. If a character in `ransomNote` is not found in `magazine`, return `false`.

**Time Complexity:** O(n * m) - Where `n` is the length of `ransomNote` and `m` is the length of `magazine`.\
**Space Complexity:** O(m) - Because of the rune slice.

```go
func canConstruct(ransomNote string, magazine string) bool {
    // Convert magazine string to a slice of runes for easy removal
    magazineRunes := []rune(magazine)
    
    for _, char := range ransomNote {
        found := false
        for i, magazineChar := range magazineRunes {
            if char == magazineChar {
                // If character matches, remove it from magazine slice
                magazineRunes = append(magazineRunes[:i], magazineRunes[i+1:]...)
                found = true
                break
            }
        }
        // If the character is not found in magazine, return false
        if !found {
            return false
        }
    }
    // All characters in ransomNote are found in magazine
    return true
}
```

---

### Intermediate Approach: Frequency Count

**Intuition:**

Instead of constantly manipulating the `magazine`, we can use an array to count the frequency of each character in the magazine and then decrement the required characters when found.

**Steps:**

1. Initialize a frequency array for characters in `magazine`.
2. Traverse `magazine` string and increment the count for each character.
3. Traverse `ransomNote` and decrement for each character.
4. If any character in `ransomNote` cannot be decremented (count is zero), return `false`.

**Time Complexity:** O(n + m) - Where `n` is the length of `ransomNote` and `m` is the length of `magazine`.\
**Space Complexity:** O(1) - Fixed array size.

```go
func canConstruct(ransomNote string, magazine string) bool {
    // Create an array to store frequencies of 26 lowercase letters
    frequency := [26]int{}
    
    // Count each character in magazine
    for _, char := range magazine {
        frequency[char-'a']++
    }
    
    // Try to form ransomNote from magazine
    for _, char := range ransomNote {
        if frequency[char-'a'] == 0 {
            return false
        }
        frequency[char-'a']--
    }
    
    return true
}
```

---

### Optimal Approach: Using Go's Map

**Intuition:**

Using a map in Go provides a more flexible and understandable way to keep track of character counts without restricting the solution to only lowercase English letters, making this approach more scalable.

**Steps:**

1. Create a map to count the frequency of each character in the `magazine`.
2. For each character in `ransomNote`, decrement its count in the map.
3. If a character is not available (count is zero) or doesn't exist in the map, return `false`.

**Time Complexity:** O(n + m) - Where `n` is the length of `ransomNote` and `m` is the length of `magazine`.\
**Space Complexity:** O(m) - Map size based on distinct characters in `magazine`.

```go
func canConstruct(ransomNote string, magazine string) bool {
    // Initialize a map for frequency count
    frequency := make(map[rune]int)
    
    // Count frequencies in the magazine string
    for _, char := range magazine {
        frequency[char]++
    }
    
    // Check each character in ransomNote
    for _, char := range ransomNote {
        if frequency[char] == 0 {
            return false
        }
        frequency[char]--
    }
    // All characters in ransomNote are met
    return true
}
```

These solutions provide a range of approaches from simple brute force to optimal map usage, allowing understanding of different strategies for problem-solving on programming platforms such as LeetCode.

