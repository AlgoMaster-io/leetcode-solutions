# [LeetCode Problem 49: Group Anagrams](https://leetcode.com/problems/group-anagrams/)

## Approaches
1. [Brute Force - Sorting and Comparing](#brute-force---sorting-and-comparing)
2. [Using HashMap with Sorted Strings as Keys](#using-hashmap-with-sorted-strings-as-keys)
3. [Optimized HashMap with Character Count as Key](#optimized-hashmap-with-character-count-as-key)

## Brute Force - Sorting and Comparing

### Intuition
The basic idea is to sort every string and then compare sorted strings to group them. If two strings are anagrams, sorting them will result in the same string. We can maintain a list of groups and add each new string to an existing group if its sorted version matches.

### Code
```go
func groupAnagrams(strs []string) [][]string {
    var groups [][]string
    var seen []string

    for _, str := range strs {
        sortedStr := sortString(str)
        found := false
        for i, s := range seen {
            if s == sortedStr {
                groups[i] = append(groups[i], str)
                found = true
                break
            }
        }
        if !found {
            groups = append(groups, []string{str})
            seen = append(seen, sortedStr)
        }
    }

    return groups
}

func sortString(str string) string {
    s := []rune(str)
    sort.Slice(s, func(i, j int) bool {
        return s[i] < s[j]
    })
    return string(s)
}
```

### Time and Space Complexity
- **Time Complexity**: O(N * KlogK), where N is the number of strings in the input and K is the maximum length of a string. For each string, we are sorting it, which takes O(KlogK).
- **Space Complexity**: O(NK), which is the space used by the seen slice and groups.

## Using HashMap with Sorted Strings as Keys

### Intuition
Instead of manually managing lists and checks, use a hash map. The key will be the sorted string. All anagrams, when sorted, yield the same string, hence can be grouped using this key.

### Code
```go
func groupAnagrams(strs []string) [][]string {
    anagramMap := make(map[string][]string)

    for _, str := range strs {
        sortedStr := sortString(str)
        anagramMap[sortedStr] = append(anagramMap[sortedStr], str)
    }

    var groups [][]string
    for _, group := range anagramMap {
        groups = append(groups, group)
    }

    return groups
}

func sortString(str string) string {
    s := []rune(str)
    sort.Slice(s, func(i, j int) bool {
        return s[i] < s[j]
    })
    return string(s)
}
```

### Time and Space Complexity
- **Time Complexity**: O(N * KlogK), where N is the number of strings, and K is the length of each string (for sorting).
- **Space Complexity**: O(NK), to store the hash map.

## Optimized HashMap with Character Count as Key

### Intuition
Instead of sorting the string, count each character's frequency (since input is limited to lowercase letters). This gives a representation unique to each anagram set, which makes a suitable hash map key.

### Code
```go
func groupAnagrams(strs []string) [][]string {
    anagramMap := make(map[[26]int][]string)

    for _, str := range strs {
        charCount := [26]int{}
        for _, char := range str {
            charCount[char-'a']++
        }
        anagramMap[charCount] = append(anagramMap[charCount], str)
    }

    var groups [][]string
    for _, group := range anagramMap {
        groups = append(groups, group)
    }

    return groups
}
```

### Time and Space Complexity
- **Time Complexity**: O(NK), where N is the number of strings and K is the max string length, as counting characters is O(K).
- **Space Complexity**: O(NK), for storing the hash map. Here, we maintain arrays of constant size (26 for each alphabet), so space used per string remains effectively constant across inputs.

