## [Leetcode 30: Substring with Concatenation of All Words](https://leetcode.com/problems/substring-with-concatenation-of-all-words/)

### Approaches
- [Approach 1: Brute Force Solution](#approach-1-brute-force-solution)
- [Approach 2: Sliding Window with Hashmap Solution](#approach-2-sliding-window-with-hashmap-solution)

---

### Approach 1: Brute Force Solution

**Intuition:**

The brute force approach involves checking all possible start indices of the string `s`. For each start index, determine whether starting from that index it's possible to find all words in the `words` list consecutively without any skipped characters. This is achieved by iterating through every possible part of the string `s` and counting the words.

**Steps:**

1. Calculate the length of each word in `words` and determine the total length required for a substring to be valid (`total_length`).
2. Iterate over every possible starting position `i` in `s` where a substring could start.
3. For each starting point, check if the substring contains a valid concatenation of `words`.
4. Use a dictionary to keep track of the counts of words needed and update it as we check the substring.
5. If the current substring contains all words with the correct frequency, store the starting index.

**Time Complexity:** O(n * m * l)  
- n = length of `s`
- m = number of words
- l = length of each word

**Space Complexity:** O(m)

```python
def findSubstring(s, words):
    if not s or not words:
        return []
    
    word_length = len(words[0])
    word_count = len(words)
    total_length = word_length * word_count

    word_freq = {}
    for word in words:
        if word in word_freq:
            word_freq[word] += 1
        else:
            word_freq[word] = 1

    result = []
    
    for i in range(len(s) - total_length + 1):  # start index
        seen = {}
        j = 0
        
        while j < word_count:
            # Find the word in the substring
            word_index = i + j * word_length
            current_word = s[word_index: word_index + word_length]
            
            # If word is not part of the required words, break
            if current_word not in word_freq:
                break
            
            # Update the seen word count
            if current_word in seen:
                seen[current_word] += 1
            else:
                seen[current_word] = 1
            
            # If seen more than required, break
            if seen[current_word] > word_freq[current_word]:
                break

            j += 1
        
        if j == word_count:
            result.append(i)
    
    return result
```

---

### Approach 2: Sliding Window with Hashmap Solution

**Intuition:**

This method optimizes the brute force solution by using a sliding window technique alongside hashmaps for tracking the frequency of words. Instead of checking each possible starting point from scratch, this approach leverages the overlap of checked portions of the string with a hashmap to adjust for words only when necessary.

**Steps:**

1. Use two dictionaries: One for tracking the frequency of each word (target frequency) and another for tracking the current frequency of words in the sliding window.
2. Iterate over `s` by moving in increments of `word_length`.
3. For each starting point, expand the window by `word_length` characters until a match is no longer possible or the window is too far.
4. Adjust the starting point when a word count exceeds the target frequency.
5. Record starting indices where all words are matched with correct frequency.

**Time Complexity:** O(n * l)  
- n = length of `s`
- l = length of each word

**Space Complexity:** O(m)

```python
def findSubstring(s, words):
    if not s or not words:
        return []
    
    word_length = len(words[0])
    word_count = len(words)
    total_length = word_length * word_count
    
    word_freq = {}
    for word in words:
        if word in word_freq:
            word_freq[word] += 1
        else:
            word_freq[word] = 1
    
    result = []

    # Loop through the entire string in blocks of the length of the word
    for i in range(word_length):
        left = i
        seen = {}
        count = 0

        # Go through the string in increments of word_length
        for j in range(i, len(s) - word_length + 1, word_length):
            word = s[j:j + word_length]

            # If the word is not part of the words list, reset the counters
            if word in word_freq:
                if word in seen:
                    seen[word] += 1
                else:
                    seen[word] = 1
                count += 1

                # If there are more of a word than needed, slide the left window
                while seen[word] > word_freq[word]:
                    left_word = s[left:left + word_length]
                    seen[left_word] -= 1
                    count -= 1
                    left += word_length

                # If count matches the number of words, add to result
                if count == word_count:
                    result.append(left)
            else:
                # Reset if the word is not part of the input words
                seen.clear()
                count = 0
                left = j + word_length
    
    return result
```

This more optimal solution utilizes sliding window and hashmap techniques to efficiently track and manage word counts, leading to a significant reduction in time complexity compared to the brute force method.

