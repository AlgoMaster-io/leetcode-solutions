# [Leetcode 151: Reverse Words in a String](https://leetcode.com/problems/reverse-words-in-a-string/)

## Table of Contents
1. [Simple Approach: Using Splitting and Joining](#simple-approach-using-splitting-and-joining)
2. [Optimal Approach: Two Pointers](#optimal-approach-two-pointers)

### Simple Approach: Using Splitting and Joining

**Intuition:**

The simplest way to reverse the words in a string is to leverage built-in string manipulation functions. The idea is to split the string based on spaces to extract the words, reverse the list of words, and then join them back into a single string with a single space as a delimiter.

**Steps:**

1. Trim leading and trailing spaces from the input string.
2. Split the string based on one or more spaces to extract words. This will handle multiple spaces between words naturally.
3. Reverse the list of words.
4. Join the reversed list of words with a single space.
5. Return the resulting string.

**Time Complexity:** O(n), where n is the length of the input string. This complexity arises due to the split, reverse, and join operations.

**Space Complexity:** O(n), because of the space required to store the list of words, in addition to the input string.

```go
func reverseWordsSimple(s string) string {
	// Trim spaces
	s = strings.TrimSpace(s)
	
	// Split the string by spaces
	words := strings.Fields(s)
	
	// Reverse the list of words
	for i, j := 0, len(words)-1; i < j; i, j = i+1, j-1 {
		words[i], words[j] = words[j], words[i]
	}
	
	// Join them back with a single space
	return strings.Join(words, " ")
}
```

### Optimal Approach: Two Pointers

**Intuition:**

To reverse the words in place with optimal time and space complexity, we can employ the two-pointer technique. This approach avoids the need to use auxiliary space to store separate words.

**Steps:**

1. Trim leading and trailing spaces from the input string.
2. Traverse the string from the end to the beginning to extract words.
3. Use a pointer to identify the end of each word and another to find the beginning of the word.
4. Append each identified word into a result list.
5. Join the result list with a single space to form the final reversed string.

**Time Complexity:** O(n), where n is the length of the input string. This is due to the single pass needed to form the result.

**Space Complexity:** O(n), where n is again due to the usage of the result list storing the reversed words (even though they are slices of the original string).

```go
func reverseWordsOptimal(s string) string {
    // Trim spaces
    s = strings.TrimSpace(s)
    
    // Prepare a result string
    result := []string{}
    
    i := len(s) - 1
    for i >= 0 {
        if s[i] == ' ' {
            i--
            continue
        }
        // Find the end of the word
        j := i
        // Find the beginning of the word
        for i >= 0 && s[i] != ' ' {
            i--
        }
        // Extract the word
        word := s[i+1 : j+1]
        result = append(result, word)
    }
    
    // Join the result list
    return strings.Join(result, " ")
}
```

These solutions provide both a straightforward and optimal approach to solving the problem of reversing words in a string. Remember to properly handle trimming and multiple spaces between words when implementing these solutions.

