[792. Number of Matching Subsequences](https://leetcode.com/problems/number-of-matching-subsequences/)

### Solution Approaches
1. [Brute Force Approach](#brute-force-approach)
2. [Optimized Approach Using Buckets](#optimized-approach-using-buckets)

---

### Brute Force Approach
**Intuition**:  
For each word in the list, check if it is a subsequence of the given string `s`. We achieve this by having two pointers: one iterating over the characters of `s` and another over the characters of the current word. If we find all characters of the word in `s`, the word is a subsequence.

**Steps**:
- For each word, iterate both through the word and the string `s`.
- Use a pointer `i` for `s` and `j` for the current word.
- Increment `i` until the end of `s`, and whenever `s[i]` matches `word[j]`, increment `j`.
- If `j` reaches the length of the word, it means the entire word is a subsequence of `s`.
  
**Time Complexity**:  O(n * m * |s|), where `n` is the number of words, `m` is the average length of the words, and `|s|` is the length of `s`.

**Space Complexity**: O(1), aside from input data.

```cpp
#include <iostream>
#include <vector>
#include <string>

bool isSubsequence(const std::string &s, const std::string &word) {
    int i = 0, j = 0;
    while (i < s.length() && j < word.length()) {
        if (s[i] == word[j]) {
            j++; // Move pointer on word
        }
        i++; // Always move pointer on s
    }
    return j == word.length(); // Check if we have covered the entire word
}

int numMatchingSubseq(std::string s, std::vector<std::string>& words) {
    int count = 0;
    for (auto &word : words) {
        if (isSubsequence(s, word)) {
            count++;
        }
    }
    return count;
}

```

---

### Optimized Approach Using Buckets
**Intuition**:  
The main challenge in the brute force approach is the repetitive scanning of the string `s`. Instead, we can use a bucket tracking system to efficiently track the positions of each letter in `s` that can potentially match the words.

**Steps**:
- Create a buckets array (one for each character) to store iterators of each word's character.
- Iterate over each word and for each word character, place it in the respective bucket (e.g., the first character of a word which is 'c' will go in the bucket for 'c').
- Traverse through the string `s` and for each character, check the respective bucket. If the word can progress by this character, move its iterator forward.
- If the word completes, count it as a subsequence.

**Time Complexity**: O(n * m + |s|), where `n` is the number of words, `m` is the average length of the words, and `|s|` is the length of `s`. This approach avoids reevaluation by efficiently utilizing buckets.

**Space Complexity**: O(n), where `n` is the number of words (for storing iterators).

```cpp
#include <iostream>
#include <vector>
#include <string>
#include <array>
#include <list>

int numMatchingSubseq(std::string s, std::vector<std::string>& words) {
    // Array of 26 lists of iterator pointing to words and their current character positions
    std::array<std::list<std::pair<std::string::iterator, std::string::iterator>>, 26> waiting;
    
    // Distribute words into buckets based on their first character
    for (auto &word : words) {
        waiting[word[0] - 'a'].emplace_back(word.begin(), word.end());
    }
    
    int count = 0;
    // Go through each character of `s`
    for (char c : s) {
        auto currentBucket = waiting[c - 'a'];
        waiting[c - 'a'].clear(); // Clear the current bucket as we process it
        
        // Process each word in the current bucket
        for (auto it : currentBucket) {
            auto [curr, end] = it;
            ++curr; // Move iterator to the next character
            
            if (curr == end) {
                count++; // If we've reached the end, it's a subsequence
            } else {
                waiting[*curr - 'a'].emplace_back(curr, end); // Move to corresponding bucket
            }
        }
    }
    return count;
}

```

This optimized approach significantly reduces the time required, especially beneficial when `s` is large and words are numerous.

