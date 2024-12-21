## [Leetcode Problem 30: Substring with Concatenation of All Words](https://leetcode.com/problems/substring-with-concatenation-of-all-words/)

### Approaches
1. [Brute Force](#brute-force)
2. [Optimized Sliding Window](#optimized-sliding-window)

---

### 1. Brute Force

**Intuition:**

In this approach, we'll generate all possible substrings of the input that have the same length as the total length of the concatenated words. For each substring, we'll check if all words are used exactly once. Given the constraints, it's a straightforward but inefficient solution, as it requires generating and verifying all possible substrings.

#### Steps:

1. Calculate the total length `totalLength` that the concatenation of all words will be `wordLength * numberOfWords`.
2. Iterate over every possible starting index `i` from `0` to `s.length - totalLength`.
3. For each starting index, extract the substring of length `totalLength` and split it into words of length `wordLength`.
4. Use a `map` to count the frequency of each word in the split substring and verify it against the frequency of words in the `words` array.
5. If they match, store the starting index.

#### Code:

```javascript
var findSubstring = function(s, words) {
    if (s.length === 0 || words.length === 0) return [];
    
    let wordLength = words[0].length;
    let wordCount = words.length;
    let totalLength = wordLength * wordCount;
    let result = [];
    
    // Initialize word frequency map
    let wordMap = {};
    words.forEach(word => {
        wordMap[word] = wordMap[word] ? wordMap[word] + 1 : 1;
    });
    
    // Iterate over possible start positions
    for (let i = 0; i <= s.length - totalLength; i++) {
        // Build a map for the current slice
        let seen = {};
        let j = 0;
        
        // For each word length segment in this slice
        while (j < wordCount) {
            let wordIndex = i + j * wordLength;
            let word = s.substring(wordIndex, wordIndex + wordLength);
            
            if (!wordMap[word]) {
                break;
            }
            
            seen[word] = seen[word] ? seen[word] + 1 : 1;
            
            // More appearances than needed
            if (seen[word] > wordMap[word]) {
                break;
            }
            
            j++;
        }
        
        // If all words matched
        if (j === wordCount) {
            result.push(i);
        }
    }
    
    return result;
};
```

**Time Complexity:** O((n - totalLength) * totalLength)  
**Space Complexity:** O(k) where k is the number of unique words  

---

### 2. Optimized Sliding Window

**Intuition:**

Here, we utilize a sliding window approach with two pointers and hash maps for counting the occurrences of each word. By processing the string in "blocks" of word lengths, we can efficiently control and validate the current window using frequency maps without recomputing word frequencies from scratch every time.

#### Steps:

1. Calculate the total length `totalLength` that the concatenation of all words will be `wordLength * numberOfWords`.
2. Initialize a word frequency `map` for all the given `words`.
3. Use a loop iterating over `wordLength` starting positions to ensure we’re only considering valid word alignments.
4. Inside the loop, use a sliding window (`left` and `right` pointers) to check for valid start positions.
5. Maintain a `seen` map to track word frequencies in the current window.
6. As the window slides, check if `seen` matches `wordMap`. If so, record the starting index.

#### Code:

```javascript
var findSubstring = function(s, words) {
    if (s.length === 0 || words.length === 0) return [];
    
    let wordLength = words[0].length;
    let wordCount = words.length;
    let totalLength = wordLength * wordCount;
    let result = [];
    
    let wordMap = {};
    for (let word of words) {
        wordMap[word] = (wordMap[word] || 0) + 1;
    }

    for (let i = 0; i < wordLength; i++) {
        let left = i;
        let right = i;
        let count = 0;
        let seen = {};

        while (right + wordLength <= s.length) {
            let word = s.substring(right, right + wordLength);
            right += wordLength;
            if (wordMap[word]) {
                seen[word] = (seen[word] || 0) + 1;
                count++;
                
                while (seen[word] > wordMap[word]) {
                    let leftWord = s.substring(left, left + wordLength);
                    seen[leftWord] = seen[leftWord] - 1;
                    count--;
                    left += wordLength;
                }

                if (count === wordCount) {
                    result.push(left);
                }
            } else {
                seen = {};
                count = 0;
                left = right;
            }
        }
    }
    
    return result;
};
```

**Time Complexity:** O(n) - where n is the length of s  
**Space Complexity:** O(k) where k is the number of unique words

This optimized sliding window approach significantly reduces redundancy by focusing only on valid word alignments and adjusting the window dynamically using the frequency maps.

