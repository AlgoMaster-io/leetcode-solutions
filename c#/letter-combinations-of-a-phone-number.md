# [Leetcode 17: Letter Combinations of a Phone Number](https://leetcode.com/problems/letter-combinations-of-a-phone-number/)

## Approaches:

1. [Backtracking Approach](#backtracking-approach)
2. [Iterative Approach using Queue](#iterative-approach-using-queue)

---

## Backtracking Approach

The problem statement is akin to generating all possible strings from a given digit sequence, where each digit maps to a set of letters (similar to the old phone keypads). This can be efficiently solved using a backtracking approach:

### Intuition:

Take each digit in the input, and attempt to construct all possible strings character by character. For each digit, you have a set of characters it can contribute. You recursively build these strings until you complete one full length of the input digits, adding to your results list when a full combination is formed.

```csharp
public IList<string> LetterCombinations(string digits) {
    // If the input digits string is empty, return an empty list
    if (string.IsNullOrEmpty(digits)) return new List<string>();
    
    // Dictionary to map digits to corresponding letters
    Dictionary<char, string> digitToCharMap = new Dictionary<char, string> {
        {'2', "abc"}, {'3', "def"}, {'4', "ghi"},
        {'5', "jkl"}, {'6', "mno"}, {'7', "pqrs"},
        {'8', "tuv"}, {'9', "wxyz"}
    };

    List<string> result = new List<string>();
    Backtrack(result, digitToCharMap, digits, new StringBuilder(), 0);
    return result;
}

// Helper method for backtracking
private void Backtrack(IList<string> result, Dictionary<char, string> map, string digits, StringBuilder currentCombination, int index) {
    // If the current combination length is equal to input digits length, add to result
    if (index == digits.Length) {
        result.Add(currentCombination.ToString());
        return;
    }

    // Get characters for the current digit
    string possibleChars = map[digits[index]];

    // Loop through each character and recursively build combinations
    for (int i = 0; i < possibleChars.Length; i++) {
        currentCombination.Append(possibleChars[i]);  // Add the character
        Backtrack(result, map, digits, currentCombination, index + 1); // Recursive call
        currentCombination.Length--; // Remove last character to backtrack
    }
}
```

### Time Complexity:

- O(3^N * 4^M) – Where N is the number of digits with 3 letter mappings (2, 3, 4, 5, 6, 8) and M is the number of digits with 4 letter mappings (7, 9).
- For each digit, there are 3 or 4 choices, and the algorithm explores all combinations.

### Space Complexity:

- O(3^N * 4^M) – To store the output.

---

## Iterative Approach using Queue

Instead of using recursion, we can iteratively build combinations using a queue. This approach can be more intuitive since it simulates exploring the entire level of combinations at each step.

### Intuition:

Start with an empty string in a queue. For each digit in the input, dequeue the elements of the current level and enqueue each possibility derived by adding new characters corresponding to the current digit.

```csharp
public IList<string> LetterCombinationsIterative(string digits) {
    if (string.IsNullOrEmpty(digits)) return new List<string>();

    Dictionary<char, string> digitToCharMap = new Dictionary<char, string> {
        {'2', "abc"}, {'3', "def"}, {'4', "ghi"},
        {'5', "jkl"}, {'6', "mno"}, {'7', "pqrs"},
        {'8', "tuv"}, {'9', "wxyz"}
    };

    Queue<string> queue = new Queue<string>();
    queue.Enqueue(""); // Start with an empty string

    foreach (char digit in digits) {
        string possibleChars = digitToCharMap[digit];

        // Process all elements currently in the queue
        int currentQueueSize = queue.Count;
        for (int i = 0; i < currentQueueSize; i++) {
            string combination = queue.Dequeue();

            // Enqueue new combinations
            foreach (char c in possibleChars) {
                queue.Enqueue(combination + c);
            }
        }
    }

    return queue.ToList();
}
```

### Time Complexity:

- O(3^N * 4^M) – Similar reasoning as the backtracking solution.

### Space Complexity:

- O(3^N * 4^M) – For storing the elements in the queue.

Choose the iterative or recursive approach based on your need for simplicity (iterative is often easier to follow) or resource constraints (recursion might use more stack space). 

Both approaches effectively solve the problem, offering a clear and complete exploration of the solution space for the input sequence.

