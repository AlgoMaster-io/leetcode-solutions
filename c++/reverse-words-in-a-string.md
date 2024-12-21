# [Leetcode 151: Reverse Words in a String](https://leetcode.com/problems/reverse-words-in-a-string/)

## Approaches:
- [Naive Approach](#naive-approach)
- [Trim, Reverse and Join](#trim-reverse-and-join)

### Naive Approach

**Intuition:**

The naive method involves using string streams to extract the words from the string one by one and then reverse them. This can be a straightforward method to implement the solution by leveraging string handling capabilities in C++.

**Steps:**
1. Use a string stream to read the input string.
2. Push each word into a vector of strings.
3. Reverse the vector to reorder the words in reverse.
4. Use additional string operations to concatenate the words back into a result.

**Code:**

```cpp
#include <iostream>
#include <sstream>
#include <vector>

std::string reverseWords(std::string s) {
    std::stringstream ss(s);
    std::string word;
    std::vector<std::string> words;

    // Extract words from stringstream
    while (ss >> word) {
        words.push_back(word);
    }

    // Reverse the order of words in the vector
    std::reverse(words.begin(), words.end());

    // Prepare and return the result
    std::string result;
    for (int i = 0; i < words.size(); i++) {
        if (i > 0) {
            result += " ";
        }
        result += words[i];
    }
    return result;
}

int main() {
    std::string s = "the sky is blue";
    std::cout << reverseWords(s) << std::endl;  // Output: "blue is sky the"
    return 0;
}
```

**Time Complexity:** O(n), where n is the length of the string `s`. We process each character a limited number of times.

**Space Complexity:** O(n), since we use a vector to store the words and additional storage to build the result.

### Trim, Reverse and Join

**Intuition:**

This approach employs string manipulation directly using `std::string` operations. The core idea is to trim unnecessary spaces, reverse the whole string, reverse each word back while fixing spaces.

**Steps:**
1. Trim leading and trailing spaces from the string.
2. Reverse the entire string to bring the words in reverse order but with the words themselves reversed.
3. Traverse the string again to reverse each individual word to its correct ordering while being in the right position.
4. Ensure single space separation between words during the joining step.

**Code:**

```cpp
#include <iostream>

std::string reverseWords(std::string s) {
    // Trim leading, trailing, and multiple spaces
    int n = s.size();
    int begin = 0, end = n - 1;
    
    // Remove leading spaces
    while (begin <= end && s[begin] == ' ') begin++;
    // Remove trailing spaces
    while (begin <= end && s[end] == ' ') end--;
    
    std::string result;
    while (begin <= end) {
        if (s[begin] != ' ') {
            result += s[begin];
        } else if (result.back() != ' ') {
            result += ' ';
        }
        begin++;
    }
    
    // Reverse the entire string
    std::reverse(result.begin(), result.end());

    // Reverse each word in the reversed string
    int l = 0;
    for (int r = 0; r <= result.size(); r++) {
        if (r == result.size() || result[r] == ' ') {
            std::reverse(result.begin() + l, result.begin() + r);
            l = r + 1;
        }
    }
    
    return result;
}

int main() {
    std::string s = "  hello world  ";
    std::cout << reverseWords(s) << std::endl; // Output: "world hello"
    return 0;
}
```

**Time Complexity:** O(n), where n is the length of the string `s`. We scan through the string a constant number of times.

**Space Complexity:** O(1), if we consider string operations modifying the string in place beyond the space needed for the result. Otherwise, it's O(n) due to storing the trimmed version.

These solutions provide a thorough understanding from naive implementation to a more space-efficient solution that reshuffles the words by intelligent string manipulation.

