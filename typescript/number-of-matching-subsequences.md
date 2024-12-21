# [Leetcode 792: Number of Matching Subsequences](https://leetcode.com/problems/number-of-matching-subsequences/)

## Approaches
1. [Brute Force Approach](#brute-force-approach)
2. [Optimized Approach Using Buckets](#optimized-approach-using-buckets)

### Brute Force Approach

#### Intuition:
The brute force approach involves checking each word in the list to see if it is a subsequence of the given string `s`. This involves iterating through the string `s` and the word, and checking if the characters can be matched in sequence.

#### Steps:
1. For each word, initialize two pointers: one for `s` and one for the word.
2. Traverse the string `s` with the pointer.
3. If the current character of `s` matches the current character of the word, move both pointers forward.
4. If the pointer of the word reaches the end of the word, it means the word is a subsequence of `s`.
5. Count how many words are subsequences of `s`.

#### Code:
```typescript
function isSubsequence(s: string, word: string): boolean {
  let sIndex = 0, wIndex = 0;
  
  while (sIndex < s.length && wIndex < word.length) {
    // If the character matches, move both pointers
    if (s[sIndex] === word[wIndex]) {
      wIndex++;
    }
    // Always move the s pointer
    sIndex++;
  }
  
  // If we have checked all characters of the word
  return wIndex === word.length;
}

function numMatchingSubseq(s: string, words: string[]): number {
  let count = 0;
  
  // Check for each word if it is a subsequence of s
  for (let word of words) {
    if (isSubsequence(s, word)) {
      count++;
    }
  }
  
  return count;
}
```

#### Time and Space Complexity:
- **Time Complexity**: \(O(N \times \text{len}(S))\), where \(N\) is the number of words and \(\text{len}(S)\) is the length of the string `s`.
- **Space Complexity**: \(O(1)\)

### Optimized Approach Using Buckets

#### Intuition:
Instead of repeatedly checking each word against `s`, we can use a hashmap (buckets) to group words starting with the same letter together. By traversing `s` only once and processing these groups, we can efficiently determine which words are subsequences.

#### Steps:
1. Initialize an array of 26 buckets for each letter of the alphabet.
2. Place each word in the appropriate bucket based on its first character.
3. Traverse the string `s`. For each character, move all words from the bucket corresponding to that character forward. If a word is complete, it is a subsequence.
4. Continue this process for each character in `s`.

#### Code:
```typescript
function numMatchingSubseq(s: string, words: string[]): number {
  const aCharCode = 'a'.charCodeAt(0);
  const buckets: string[][] = Array.from({ length: 26 }, () => []);
  
  // Place words in the bucket corresponding to their first character
  for (const word of words) {
    const index = word.charCodeAt(0) - aCharCode;
    buckets[index].push(word);
  }

  let count = 0;

  // Traverse the string s
  for (const char of s) {
    const index = char.charCodeAt(0) - aCharCode;
    // Extract current bucket
    const currentBucket = buckets[index];
    buckets[index] = []; // Clear current bucket
    
    // Process words in current bucket
    for (const word of currentBucket) {
      if (word.length === 1) {
        count++; // We've matched the whole word
      } else {
        // Move the rest of the word to the bucket of the next character
        const nextBucketIndex = word.charCodeAt(1) - aCharCode;
        buckets[nextBucketIndex].push(word.slice(1));
      }
    }
  }

  return count;
}
```

#### Time and Space Complexity:
- **Time Complexity**: \(O(N + \text{len}(S))\), where \(N\) is the total length of all words and \(\text{len}(S)\) is the length of the string `s`.
- **Space Complexity**: \(O(N)\), for storing the input words.

