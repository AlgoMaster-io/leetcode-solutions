## [Leetcode 30: Substring with Concatenation of All Words](https://leetcode.com/problems/substring-with-concatenation-of-all-words/)

### Approaches
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Sliding Window with Hash Maps](#approach-2-sliding-window-with-hash-maps)

### Approach 1: Brute Force

#### Intuition:
The problem requires us to find the starting indices of substrings in `s` that are a concatenation of all the words in `words` list exactly once and without any intervening characters. A brute force approach would involve checking every possible starting position in `s` and then seeing if a valid concatenation starts there.

#### Detailed Explanation:
1. Calculate the total length required for a valid concatenation (based on the length of each word and the count of words).
2. Iterate over every possible starting point in `s` where a substring of the required concatenation length can fit.
3. For each starting index, extract a substring of the required length and check if it contains all words from the `words` list exactly once by using a word count comparison.

#### Time Complexity:
- **O((n - totalLen + 1) * (m * k))**, where `n` is the length of `s`, `m` is the number of words, and `k` is the average length of words.
  
#### Space Complexity:
- **O(m)**, for storing the word and frequency counts.

```typescript
function findSubstring(s: string, words: string[]): number[] {
    const wordLen = words[0].length;
    const numWords = words.length;
    const totalLen = wordLen * numWords;
    
    const wordCount = new Map<string, number>();
    words.forEach(word => wordCount.set(word, (wordCount.get(word) || 0) + 1));
    
    const result: number[] = [];
    
    for (let i = 0; i <= s.length - totalLen; i++) {
        const seen = new Map<string, number>();
        let j = 0;
        while (j < numWords) {
            const wordIndex = i + j * wordLen;
            const word = s.substr(wordIndex, wordLen);
            if (wordCount.has(word)) {
                seen.set(word, (seen.get(word) || 0) + 1);
                if (seen.get(word) > wordCount.get(word)!) {
                    break;
                }
            } else {
                break;
            }
            j++;
        }
        if (j === numWords) {
            result.push(i);
        }
    }
    
    return result;
}
```

### Approach 2: Sliding Window with Hash Maps

#### Intuition:
A more optimal approach leverages the sliding window technique. Instead of recalculating everything for each starting index, we slide the window across the string `s` while maintaining counts of words. 

#### Detailed Explanation:
1. Divide the task into word size windows, starting at each offset from 0 to `wordLen -1`.
2. For each window, keep track of the current count of valid words that are in the correct sequence.
3. Use two pointers to expand and contract the window while adjusting counts of seen words.
4. If the window contains exactly the required number of each word, record the starting index.

#### Time Complexity:
- **O(n * k)**, where `k` is the length of each word and `n` is the length of the string `s`.
  
#### Space Complexity:
- **O(m)**, for storing the word and frequency counts.

```typescript
function findSubstring(s: string, words: string[]): number[] {
    const wordLen = words[0].length;
    const numWords = words.length;
    const totalLen = wordLen * numWords;
    const n = s.length;

    const result: number[] = [];
    const wordCount = new Map<string, number>();
    words.forEach(word => wordCount.set(word, (wordCount.get(word) || 0) + 1));

    for (let i = 0; i < wordLen; i++) {
        let left = i;
        let count = 0;
        const seen = new Map<string, number>();

        for (let j = i; j <= n - wordLen; j += wordLen) {
            const word = s.substring(j, j + wordLen);
 
            if (wordCount.has(word)) {
                seen.set(word, (seen.get(word) || 0) + 1);
                count++;

                while (seen.get(word)! > wordCount.get(word)!) {
                    const leftWord = s.substring(left, left + wordLen);
                    seen.set(leftWord, seen.get(leftWord)! - 1);
                    left += wordLen;
                    count--;
                }

                if (count === numWords) {
                    result.push(left);
                }
            } else {
                seen.clear();
                count = 0;
                left = j + wordLen;
            }
        }
    }

    return result;
}
```

By examining and addressing the string by windows of the word length, this approach effectively reduces redundant calculations, making the algorithm more efficient.

