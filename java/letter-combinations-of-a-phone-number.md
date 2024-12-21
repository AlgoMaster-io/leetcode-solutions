# [Leetcode 17: Letter Combinations of a Phone Number](https://leetcode.com/problems/letter-combinations-of-a-phone-number/)

## Approaches

- [Backtracking Approach](#backtracking-approach)

### Backtracking Approach

#### Intuition

To solve the problem of generating all possible letter combinations given a string of digits mapped to letters as on a telephone keypad, we can use backtracking. This approach explores all possible solutions by building each combination step by step and then concatenating letters that map to each digit.

Each digit can map to a set of letters ("2" maps to "abc", "3" maps to "def", etc.), and the problem is to generate all permutations of these letters, constrained by the order of digits in the input. 

When using backtracking:

- We recursively build each combination with the current letter, append it to our temporary string, and when the temporary string is the same length as the input digits, we have a full valid combination.
- Then we backtrack to substitute the last added letter with the others mapped by the same digit.

This method is efficient as it avoids constructing unnecessary paths once a valid one is found.

#### Java Code

```java
import java.util.ArrayList;
import java.util.List;

public class Solution {
    // Map of digits to respective letters
    private static final String[] KEYPAD = {
        "",     // 0 - no mapping
        "",     // 1 - no mapping
        "abc",  // 2
        "def",  // 3
        "ghi",  // 4
        "jkl",  // 5
        "mno",  // 6
        "pqrs", // 7
        "tuv",  // 8
        "wxyz"  // 9
    };
    
    public List<String> letterCombinations(String digits) {
        List<String> result = new ArrayList<>();
        // Return empty list if digits is empty
        if (digits == null || digits.length() == 0) {
            return result;
        }
        // Start backtracking from an empty current combination at digit index 0
        backtrack(result, new StringBuilder(), digits, 0);
        return result;
    }
    
    private void backtrack(List<String> result, StringBuilder current, String digits, int index) {
        // Base case: if the current length matches digits length, add to results
        if (index == digits.length()) {
            result.add(current.toString());
            return;
        }
        
        // Get the letters mapped to the current digit
        String letters = KEYPAD[digits.charAt(index) - '0'];
        
        // Traverse each mapped letter
        for (char letter : letters.toCharArray()) {
            // Append the letter to current combination
            current.append(letter);
            // Move to the next digit
            backtrack(result, current, digits, index + 1);
            // Backtrack by removing the last letter added
            current.deleteCharAt(current.length() - 1);
        }
    }
}
```

#### Time and Space Complexity

- **Time Complexity**: O(3^n × 4^m), where n is the number of digits mapping to 3 letters (like '2', '3', '4', etc.) and m is the number of digits mapping to 4 letters (like '7' or '9'). This complexity arises because we generate all possible combinations.
  
- **Space Complexity**: O(3^n × 4^m), since the space required is mainly due to the storage of results in the list.

The provided backtracking code efficiently explores all potential paths and constructs valid combinations dynamically while ensuring paths are logically constrained to those matching keypad mappings.

