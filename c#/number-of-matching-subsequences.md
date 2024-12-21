# [Leetcode Problem 792: Number of Matching Subsequences](https://leetcode.com/problems/number-of-matching-subsequences/)

In this problem, we're given a string `s` and an array of words, and we need to determine how many words from the array are subsequences of the string `s`.

We'll explore various approaches, from the most straightforward to more optimal solutions:

- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Using a Map to Store Character Indices with Binary Search](#approach-2-using-a-map-to-store-character-indices-with-binary-search)

## Approach 1: Brute Force

### Intuition:
The simplest approach is to iterate through each word in the list and check if it is a subsequence of `s`. For a word to be a subsequence of `s`, each character of the word must appear in order within `s`. We can use two pointers to check if a word is a subsequence of `s`.

### Steps:
1. Initialize a counter to store the number of matching subsequences.
2. For each word, use two pointers: one to iterate through `s` and another to iterate through the word.
3. Move the pointer in `s` to find each character of the word in order.
4. If all characters are found in sequence, increment the subsequence counter.

### Code:
```csharp
public int NumMatchingSubseq(string s, string[] words) {
    int count = 0;
    
    foreach (var word in words) {
        if (IsSubsequence(s, word)) {
            count++;
        }
    }
    
    return count;
}

private bool IsSubsequence(string s, string word) {
    int i = 0, j = 0;
    
    // Use two pointers to check for subsequence
    while (i < s.Length && j < word.Length) {
        if (s[i] == word[j]) {
            j++;
        }
        i++;
    }
    
    // If j reaches the end of the word, it means all characters were found in sequence
    return j == word.Length;
}
```

### Complexity:
- **Time Complexity:** O(n * m), where `n` is the length of `s` and `m` is the total length of all words combined.
- **Space Complexity:** O(1), as no extra data structure of significant size is used beyond the loop variables.

## Approach 2: Using a Map to Store Character Indices with Binary Search

### Intuition:
This approach optimizes the search for each character of the word in `s`. Instead of scanning `s` linearly for every character, we store the indices of each character in `s` in a map. For each character in the word, we then use binary search to efficiently find the next occurrence of the character in `s`.

### Steps:
1. Use a dictionary to store lists of indices for each character in `s`.
2. For each word, check if it is a subsequence:
    - Initialize a variable for the current position in `s`.
    - For each character in the word, use binary search to find its next occurrence in `s` starting from the current position.
3. If every character in the word can be matched, increment the subsequence counter.

### Code:
```csharp
public int NumMatchingSubseq(string s, string[] words) {
    // Create a dictionary of character to list of indices
    var charIndicesMap = new Dictionary<char, List<int>>();
    
    for (int i = 0; i < s.Length; i++) {
        if (!charIndicesMap.ContainsKey(s[i])) {
            charIndicesMap[s[i]] = new List<int>();
        }
        charIndicesMap[s[i]].Add(i);
    }
    
    int count = 0;
    
    foreach (var word in words) {
        if (IsSubsequenceUsingMap(s, word, charIndicesMap)) {
            count++;
        }
    }
    
    return count;
}

private bool IsSubsequenceUsingMap(string s, string word, Dictionary<char, List<int>> map) {
    int currentPos = -1; // This holds the index in `s`
    
    foreach (var ch in word) {
        if (!map.ContainsKey(ch)) {
            return false;
        }

        var indices = map[ch];
        int nextPos = FindNextPosition(indices, currentPos);
        
        if (nextPos == -1) {
            return false;
        }
        currentPos = indices[nextPos];
    }
    
    return true;
}

private int FindNextPosition(List<int> indices, int currentPos) {
    int low = 0, high = indices.Count;
    
    // Binary search for the next position greater than currentPos
    while (low < high) {
        int mid = (low + high) / 2;
        if (indices[mid] > currentPos) {
            high = mid;
        } else {
            low = mid + 1;
        }
    }
    
    return low < indices.Count ? low : -1;
}
```

### Complexity:
- **Time Complexity:** O(n + m * log(n)), where `n` is the length of `s`, and `m` is the total length of all words combined times the binary search on average.
- **Space Complexity:** O(n), since we store the indices of every character in `s`.

