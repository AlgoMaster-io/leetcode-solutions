# [Leetcode 30: Substring with Concatenation of All Words](https://leetcode.com/problems/substring-with-concatenation-of-all-words/)

## Approaches
1. [Brute Force Approach](#brute-force-approach)
2. [Optimized Sliding Window](#optimized-sliding-window)

---

## Brute Force Approach

### Intuition
The naive approach is to consider every possible starting position for the substring and check if the substring is a concatenation of all words in the list. This requires iterating over every possible substring and checking the occurrence of each word, which results in a high computational complexity.

### Steps
1. Calculate total length of substrings needed by multiplying the length of one word by the number of words.
2. For each valid starting position of the substring in the main string `s`, extract the substring of the calculated total length.
3. Split the substring into words of the same length as the words in the list.
4. Check if the list of words from the substring is an exact match with the list of words given (by comparison of word counts).
5. If it matches, store the starting index of the substring.

### Code
```go
func findSubstring(s string, words []string) []int {
    if len(s) == 0 || len(words) == 0 || len(words[0]) == 0 {
        return []int{}
    }
    
    wordLength := len(words[0])
    totalLength := wordLength * len(words)
    wordCount := make(map[string]int)
    result := []int{}
    
    // Build frequency map for the words
    for _, word := range words {
        wordCount[word]++
    }
    
    // Iterate over all possible starting points of the substring
    for i := 0; i <= len(s) - totalLength; i++ {
        seen := make(map[string]int)
        j := 0
        // Check each word in the substring
        for ; j < len(words); j++ {
            word := s[i + j * wordLength : i + (j + 1) * wordLength]
            if count, exists := wordCount[word]; exists {
                seen[word]++
                if seen[word] > count {
                    break
                }
            } else {
                break
            }
        }
        if j == len(words) {
            result = append(result, i)
        }
    }
    return result
}
```

### Complexity
- **Time Complexity:** O((N - M*L) * M * L) where N is the length of the string `s`, M is the number of words, and L is the length of each word.
- **Space Complexity:** O(M) to store the word counts.

---

## Optimized Sliding Window

### Intuition
The optimization focuses on managing over multiple starting indices in a more efficient manner, using a sliding window technique to build and base operations on previously computed information, thus reducing redundant computations.

### Steps
1. Use multiple starting points based on the word length.
2. For each start point, slide the window across the string and check for valid substrings.
3. Adjust the window size dynamically based on the encountered invalid states, thus skipping unnecessary checks.

### Code
```go
func findSubstring(s string, words []string) []int {
    if len(s) == 0 || len(words) == 0 || len(words[0]) == 0 {
        return []int{}
    }
    
    wordLength := len(words[0])
    totalLength := wordLength * len(words)
    wordCount := make(map[string]int)
    result := []int{}
    
    for _, word := range words {
        wordCount[word]++
    }
    
    // There are L potential starting points
    for i := 0; i < wordLength; i++ {
        left := i
        right := i
        seen := make(map[string]int)
        count := 0
        
        for right + wordLength <= len(s) {
            word := s[right:right + wordLength]
            right += wordLength
            
            if _, exists := wordCount[word]; exists {
                seen[word]++
                count++
                
                for seen[word] > wordCount[word] {
                    leftWord := s[left:left + wordLength]
                    left += wordLength
                    seen[leftWord]--
                    count--
                }
                
                if count == len(words) {
                    result = append(result, left)
                }
            } else {
                // Reset the window
                seen = make(map[string]int)
                count = 0
                left = right
            }
        }
    }
    
    return result
}
```

### Complexity
- **Time Complexity:** O(N * L) where N is the length of the string `s`, and L is the length of each word. Each word check is performed in constant time by manipulating the window.
- **Space Complexity:** O(M) where M is the number of unique words in the words array to store the word counts.

