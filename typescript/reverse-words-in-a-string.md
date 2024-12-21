# [Leetcode 151: Reverse Words in a String](https://leetcode.com/problems/reverse-words-in-a-string/)

## Approaches
- [Approach 1: Split and Reverse](#approach-1-split-and-reverse)
- [Approach 2: Two Pointers with Manual Parsing](#approach-2-two-pointers-with-manual-parsing)

---

### Approach 1: Split and Reverse

**Intuition:**

This straightforward method involves utilizing built-in string functions. We start by splitting the string into words, reverse the array of words, and finally join them back together into a single string. This approach is simple and takes advantage of JavaScript's efficient built-in methods.

**Steps:**

1. **Trim any extra spaces** from the start and end of the string.
2. **Split the string** into an array of words using the space character as a delimiter. This also handles multiple spaces between words.
3. **Reverse the array** of words.
4. **Join the reversed array** of words into a single string with a single space separating each word.

**Time Complexity:** O(n), where n is the length of the string. This is because splitting, reversing, and joining operations each traverse the string once.

**Space Complexity:** O(n) because additional space is required to store the array of words.

```typescript
function reverseWords(s: string): string {
  // Step 1: Trim spaces and split the string by spaces to get words
  const words = s.trim().split(/\s+/);
  
  // Step 2: Reverse the array of words
  words.reverse();
  
  // Step 3: Join the array back to a string with a space separating each word
  return words.join(' ');
}
```

---

### Approach 2: Two Pointers with Manual Parsing

**Intuition:**

This approach involves manually parsing the string using two pointers to construct the reversed sentence without relying on built-in split or reverse functions. This is a more low-level approach that allows more control over the process, and it may be more efficient in environments where the overhead of split and reverse functions is significant.

**Steps:**

1. Initialize two pointers, `i` starting from the end of the string and `j` to find the end of each word.
2. Iterate from the end of the string to the start with the following logic:
   - Move `i` leftwards until a non-space character is found, marking the end of a word.
   - Move `j` leftwards to find the start of the word.
   - Add the word to the result string.
3. Skip any spaces between words during processing with the pointers.
4. Continue until the entire string is parsed.

**Time Complexity:** O(n), where n is the length of the string, as each character is processed at most twice.

**Space Complexity:** O(n), used to store the reversed result string.

```typescript
function reverseWordsManual(s: string): string {
  let result = '';
  let i = s.length - 1;

  while (i >= 0) {
    // Skip spaces to find the end of the next word
    while (i >= 0 && s[i] === ' ') i--;

    if (i < 0) break;

    // j will be the end of the current word
    let j = i;

    // Move i to the start of the current word
    while (i >= 0 && s[i] !== ' ') i--;

    // Extract the word between i+1 to j
    const word = s.substring(i + 1, j + 1);

    // Add the word to the result with a space
    if (result.length === 0) {
      result = word;
    } else {
      result += ' ' + word;
    }
  }
  
  return result;
}
```

In both approaches, it's crucial to manage spaces carefully, ensuring the final output does not contain leading, trailing, or multiple spaces between words. The two-pointer method is more intricate but can be preferable in certain cases, offering a clearer understanding of the reversing process without relying on high-level functions.

