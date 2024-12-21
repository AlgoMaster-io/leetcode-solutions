# [Leetcode 17: Letter Combinations of a Phone Number](https://leetcode.com/problems/letter-combinations-of-a-phone-number/)

## Approaches

1. [Backtracking Approach](#backtracking-approach)

---

### Backtracking Approach

The problem of generating all possible letter combinations for a given number list on a phone keypad can be efficiently solved using the backtracking technique. Let's break this approach down step-by-step:

#### Intuition

- A classic way to generate combinations is to use backtracking, which is a recursive approach that explores all possible combinations by choosing each letter for the current digit and proceeding to the next.
- Each digit (except 0 and 1) on a traditional phone keypad corresponds to a list of letters. For example, digit '2' maps to ['a', 'b', 'c'].
- We perform a depth-first exploration: for each digit, try all possible characters and proceed to the next digit recursively.
- Once we've constructed a string of characters of the same length as the input, a valid combination is found.

#### Steps

1. Create a map (or vector) that associates each digit with the possible letters.
2. Use a recursive function that generates combinations:
   - If the current string's length equals the digits' length, add it to the results.
   - Otherwise, append each letter corresponding to the current digit and recursively handle the next.

#### Code Implementation

```cpp
#include <vector>
#include <string>
#include <unordered_map>

using namespace std;

class Solution {
public:
    vector<string> letterCombinations(string digits) {
        if (digits.empty()) return {}; // If input is empty, return an empty array
        
        // Mapping digits to the corresponding letters.
        const vector<string> keypad = {
            "",     // 0
            "",     // 1
            "abc",  // 2
            "def",  // 3
            "ghi",  // 4
            "jkl",  // 5
            "mno",  // 6
            "pqrs", // 7
            "tuv",  // 8
            "wxyz"  // 9
        };
        
        vector<string> results;
        string currentCombination;
        
        // Starting backtracking from index 0
        backtrack(digits, 0, keypad, currentCombination, results);
        
        return results;
    }

private:
    void backtrack(const string& digits, int index, const vector<string>& keypad, string& currentCombination, vector<string>& results) {
        if (index == digits.size()) {
            results.push_back(currentCombination); // Found a valid combination
            return;
        }
        
        int digit = digits[index] - '0'; // Convert char to int
        const string& possibleChars = keypad[digit]; // Get possible letters for current digit
        
        for (char ch : possibleChars) {
            currentCombination.push_back(ch); // Choose a character for current digit
            backtrack(digits, index + 1, keypad, currentCombination, results); // Move to the next digit
            currentCombination.pop_back(); // Remove the character before trying the next option
        }
    }
};
```

#### Complexity Analysis

- **Time Complexity:** O(4^n), where n is the length of the input string `digits`. Each digit could potentially map to either 3 or 4 letters.
- **Space Complexity:** O(n), where n is the length of `digits`, due to the recursive call stack in use and the auxiliary space for `currentCombination`. The space for `results` is not considered as it is required to store the output. 

This is an elegant recursive solution that concisely generates all combinations a digit string can represent by traversing the constraints a traditional phone pad imposes.

