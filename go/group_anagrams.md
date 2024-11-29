# [49. Group Anagrams](https://leetcode.com/problems/group-anagrams/)

## Approach 1: Sorting + HashMap

### Solution
```go
// Time Complexity: O(n * k * log k) where n is the number of strings and k is the average string length
// Space Complexity: O(n * k)
import "sort"

func groupAnagrams(strs []string) [][]string {
    // Map to group anagrams by their sorted representation
    mapAnagrams := make(map[string][]string)

    for _, str := range strs {
        // Sort the string to create a key for anagrams
        charArray := []rune(str)
        sort.Slice(charArray, func(i, j int) bool {
            return charArray[i] < charArray[j]
        })
        sortedStr := string(charArray)

        // Add the string to the corresponding group
        mapAnagrams[sortedStr] = append(mapAnagrams[sortedStr], str)
    }

    // Prepare the result from the map values
    result := make([][]string, 0, len(mapAnagrams))
    for _, group := range mapAnagrams {
        result = append(result, group)
    }
    
    return result
}
```

## Approach 2: Frequency Count + HashMap

### Solution
```go
// Time Complexity: O(n * k) where n is the number of strings and k is the average string length
// Space Complexity: O(n * k)
func groupAnagrams(strs []string) [][]string {
    // Map to group anagrams by their frequency count representation
    mapAnagrams := make(map[string][]string)

    for _, str := range strs {
        // Create a frequency count array for the string
        count := make([]int, 26)
        for _, c := range str {
            count[c-'a']++
        }

        // Convert frequency array to a string key
        keyBuilder := make([]byte, 0, 52)
        for _, freq := range count {
            keyBuilder = append(keyBuilder, byte(freq)+'a', '#')
        }
        key := string(keyBuilder)

        // Add the string to the corresponding group
        mapAnagrams[key] = append(mapAnagrams[key], str)
    }

    // Prepare the result from the map values
    result := make([][]string, 0, len(mapAnagrams))
    for _, group := range mapAnagrams {
        result = append(result, group)
    }
    
    return result
}
```

## Approach 3: Prime Product Hashing (Alternative)

### Solution
```go
// Time Complexity: O(n * k) where n is the number of strings and k is the average string length
// Space Complexity: O(n * k)
func groupAnagrams(strs []string) [][]string {
    // Assign a unique prime number to each character
    primes := []int{2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37, 41, 43, 47, 
                    53, 59, 61, 67, 71, 73, 79, 83, 89, 97, 101}
    mapAnagrams := make(map[int64][]string)

    for _, str := range strs {
        var product int64 = 1
        for _, c := range str {
            product *= int64(primes[c-'a']) // Compute unique hash using prime products
        }

        // Add the string to the corresponding group
        mapAnagrams[product] = append(mapAnagrams[product], str)
    }

    // Prepare the result from the map values
    result := make([][]string, 0, len(mapAnagrams))
    for _, group := range mapAnagrams {
        result = append(result, group)
    }
    
    return result
}
```

