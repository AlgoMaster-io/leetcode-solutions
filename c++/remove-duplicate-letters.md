# [Leetcode 316: Remove Duplicate Letters](https://leetcode.com/problems/remove-duplicate-letters/)

## Contents
- [Approach 1: Recursive Backtracking](#approach-1-recursive-backtracking)
- [Approach 2: Iterative Greedy using Stack](#approach-2-iterative-greedy-using-stack)

## Approach 1: Recursive Backtracking

### Intuition
The problem can be approached using a recursive backtracking algorithm. The main idea here is to build the smallest possible string that preserves the order of characters from the original string, while ensuring each letter appears only once.

We start by selecting the smallest lexicographical character and then recursively solve the subproblem by considering subsequent characters after the first occurrence of the selected character. The resulting string will be constructed recursively.

### Code
```cpp
#include <iostream>
#include <string>
#include <algorithm>
#include <unordered_map>

std::string removeDuplicateLetters(std::string s) {
    if (s.length() == 0) return "";
    
    std::unordered_map<char, int> last_occurrence;
    for (int i = 0; i < s.length(); i++) {
        last_occurrence[s[i]] = i;
    }

    std::string result;
    int start = 0;
    int end = findMinLastOccurrence(last_occurrence);

    while (end != -1) {
        char min_char = s[start];
        for (int i = start; i <= end; i++) {
            if (s[i] < min_char) {
                min_char = s[i];
                start = i;
            }
        }
        
        result += min_char;
        start++;

        last_occurrence.erase(min_char);
        end = findMinLastOccurrence(last_occurrence, start);
    }

    return result;
}

// Helper function to find the minimum last occurrence
int findMinLastOccurrence(const std::unordered_map<char, int>& last_occurrence, int start=0) {
    int min_last = -1;
    for (const auto& kv : last_occurrence) {
        if (min_last == -1 || kv.second < min_last) {
            if (kv.second >= start) {
                min_last = kv.second;
            }
        }
    }
    return min_last;
}

int main() {
    std::string s = "cbacdcbc";
    std::cout << removeDuplicateLetters(s) << std::endl;  // Output: "acdb"
    return 0;
}
```

### Time Complexity
- The time complexity is O(n^2) due to the nested operations which are repeated for each character in the worst case.
  
### Space Complexity
- The space complexity is O(n) due to usage of extra data structures like the map to store last occurrences.


## Approach 2: Iterative Greedy using Stack

### Intuition
The recursive method can be inefficient; hence a greedy approach using a stack can give us an optimal solution. In this method, we ensure the sequence is in lexicographical order by maintaining a stack and making decisions on whether to pop the top element based on future availability in the string. We also ensure that each character is included only once using a visited flag array. 

Steps:
1. Traverse through each character in the string while maintaining a frequency count of remaining characters.
2. Use a stack to build the resultant string.
3. Before adding a new character, we need to pop characters from the top of the stack if they are greater than the current character and also have remaining occurrences elsewhere in the string.
4. Ensure characters are included only once using a visited flag.

### Code
```cpp
#include <iostream>
#include <string>
#include <unordered_map>
#include <vector>
#include <stack>

std::string removeDuplicateLetters(std::string s) {
    std::unordered_map<char, int> freq;
    std::vector<bool> visited(26, false);
    std::stack<char> stack;
    
    // Get frequency of each character
    for (char c : s) {
        freq[c]++;
    }
    
    for (char c : s) {
        freq[c]--;
        
        if (visited[c - 'a']) continue;
        
        while (!stack.empty() && stack.top() > c && freq[stack.top()] > 0) {
            visited[stack.top() - 'a'] = false;
            stack.pop();
        }
        
        stack.push(c);
        visited[c - 'a'] = true;
    }
    
    std::string result;
    while (!stack.empty()) {
        result = stack.top() + result;
        stack.pop();
    }
    
    return result;
}

int main() {
    std::string s = "cbacdcbc";
    std::cout << removeDuplicateLetters(s) << std::endl;  // Output: "acdb"
    return 0;
}
```

### Time Complexity
- The time complexity is O(n) since each character is pushed and popped from the stack at most once.
  
### Space Complexity
- The space complexity is O(1) as we are utilizing a fixed array for visited flags and other auxiliary structures are bounded by constant factors.

