# [Generate Parentheses - LeetCode](https://leetcode.com/problems/generate-parentheses/)

## Approaches
1. [Brute Force - Generate All Combinations](#approach1)
2. [Backtracking](#approach2)

### Approach 1: Brute Force - Generate All Combinations

The idea is to generate all combinations of `2n` parentheses and filter out the invalid combinations.

**Intuition:**
- Generate all possible combinations of `2n` parentheses.
- Validate each combination to check if it's a valid parentheses sequence.
- A valid parentheses sequence has a balanced order of opening (`(`) and closing (`)`) brackets such that we never close more `)` than we open `(` at any point.

**Code:**
```csharp
public IList<string> GenerateParenthesis(int n) {
    List<string> result = new List<string>();
    GenerateAllCombinations(new char[2 * n], 0, result);
    return result;
}

private void GenerateAllCombinations(char[] current, int pos, List<string> result) {
    if (pos == current.Length) {
        if (IsValid(current)) {
            result.Add(new string(current));
        }
    } else {
        current[pos] = '(';
        GenerateAllCombinations(current, pos + 1, result);
        current[pos] = ')';
        GenerateAllCombinations(current, pos + 1, result);
    }
}

private bool IsValid(char[] current) {
    int balance = 0;
    foreach (char c in current) {
        if (c == '(') balance++;
        else balance--;
        if (balance < 0) return false;
    }
    return balance == 0;
}
```

**Complexity Analysis:**
- **Time Complexity:** \( O(2^{2n} n) \) - Generate each sequence and validate.
- **Space Complexity:** \( O(2n) \) for the recursion stack and storing sequences.

### Approach 2: Backtracking

Instead of generating all combinations and then filtering valid ones, we can construct valid combinations using backtracking.

**Intuition:**
- We can construct valid sequences by maintaining counts of the number of opening and closing brackets.
- A valid sequence must not close a parenthesis before it opens one. Hence the number of closing parentheses should not exceed the number of opening parentheses at any time.

**Code:**
```csharp
public IList<string> GenerateParenthesis(int n) {
    List<string> result = new List<string>();
    Backtrack(result, "", 0, 0, n);
    return result;
}

private void Backtrack(List<string> result, string current, int open, int close, int max) {
    // Base case, valid sequence constructed
    if (current.Length == max * 2) {
        result.Add(current);
        return;
    }
    
    // Add an opening bracket if we can
    if (open < max) {
        Backtrack(result, current + "(", open + 1, close, max);
    }
    
    // Add a closing bracket if valid
    if (close < open) {
        Backtrack(result, current + ")", open, close + 1, max);
    }
}
```

**Complexity Analysis:**
- **Time Complexity:** \( O(4^n / \sqrt{n}) \) - Catalan number generation.
- **Space Complexity:** \( O(n) \) for the recursion stack.

