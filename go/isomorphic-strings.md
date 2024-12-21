# [205. Isomorphic Strings](https://leetcode.com/problems/isomorphic-strings/)

## Approaches
- [Approach 1: Basic Approach with Two Hash Maps](#approach-1-basic-approach-with-two-hash-maps)
- [Approach 2: Optimized Approach with One Hash Map and Set](#approach-2-optimized-approach-with-one-hash-map-and-set)

### Approach 1: Basic Approach with Two Hash Maps

**Intuition:**

The fundamental requirement for two strings to be isomorphic is that every character in the first string should be able to map to a character in the second string uniquely and consistently. That means:

- The first string's characters should map to the second string's characters without ambiguity.
- The mapping should be bidirectional because if character 'a' in the first string maps to 'b' in the second string, then wherever 'b' appears in the second string, it should map back to 'a'.

To achieve this, we can employ two hash maps:
- `mapST`: To map characters from the first string `s` to the second string `t`.
- `mapTS`: To map characters from the second string `t` to the first string `s`.

**Algorithm:**

1. Iterate through the characters of the strings `s` and `t` simultaneously.
2. For each character pair `(charS, charT)`:
   - Check if `charS` is already mapped in `mapST`. If it is, verify that its mapped value corresponds to `charT`; if not, return `false`.
   - Check if `charT` is already mapped in `mapTS`. If it is, check that its mapped value corresponds to `charS`; if not, return `false`.
   - If neither check fails, establish the mappings `charS -> charT` in `mapST` and `charT -> charS` in `mapTS`.

**Time Complexity:** O(n), where n is the length of the strings, since each string is iterated over once.
**Space Complexity:** O(n), due to the additional space required for the two hash maps.

```go
func isIsomorphic(s string, t string) bool {
    mapST := make(map[byte]byte) // Mapping from s to t
    mapTS := make(map[byte]byte) // Mapping from t to s

    for i := 0; i < len(s); i++ {
        charS, charT := s[i], t[i]

        // Check if charS is already mapped
        if val, exists := mapST[charS]; exists {
            if val != charT {
                return false
            }
        } else {
            mapST[charS] = charT
        }

        // Check if charT is already mapped
        if val, exists := mapTS[charT]; exists {
            if val != charS {
                return false
            }
        } else {
            mapTS[charT] = charS
        }
    }
    return true
}
```

### Approach 2: Optimized Approach with One Hash Map and Set

**Intuition:**

While the two-hash-map solution works well, we can further optimize the space usage by using:
- A single hash map to track the mapping from `s` to `t`.
- A set to ensure that no two characters in `s` map to the same character in `t`.

**Algorithm:**

1. Use a single hash map `mapST` to record the map from `s` to `t`.
2. Use a set `mappedChars` to keep track of characters in `t` that have already been mapped to.
3. Iterate through each pair of characters `(charS, charT)`:
   - If `charS` exists in `mapST`, check its mapped value matches `charT`. If not, return `false`.
   - If `charS` does not exist, check if `charT` is already in `mappedChars`. If so, return `false` to prevent duplicate mappings.
   - Otherwise, update `mapST` with `charS -> charT` and add `charT` to `mappedChars`.

**Time Complexity:** O(n), where n is the length of the strings, due to a single iteration.
**Space Complexity:** O(n), for the hash map and set usage.

```go
func isIsomorphic(s string, t string) bool {
    mapST := make(map[byte]byte)  // Mapping from s to t
    mappedChars := make(map[byte]bool) // Characters in t that are already mapped

    for i := 0; i < len(s); i++ {
        charS, charT := s[i], t[i]

        // If charS is already mapped
        if mappedChar, exists := mapST[charS]; exists {
            if mappedChar != charT {
                return false
            }
        } else {
            // Check if charT is already a map target
            if mappedChars[charT] {
                return false
            }
            // Establish the mapping from charS to charT
            mapST[charS] = charT
            mappedChars[charT] = true
        }
    }
    return true
}
```

By understanding both approaches, we ensure a comprehensive grasp of the problem, enabling the selection of the most suitable solution based on the scenario's constraints.

