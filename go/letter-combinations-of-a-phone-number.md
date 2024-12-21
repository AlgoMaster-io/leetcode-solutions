# [Letter Combinations of a Phone Number](https://leetcode.com/problems/letter-combinations-of-a-phone-number/)

## Approaches

1. [Recursive Backtracking](#recursive-backtracking)
2. [Iterative with Queue](#iterative-with-queue)

---

### Recursive Backtracking

The problem is essentially about generating combinations of strings based on digit mappings, much like on a telephone keypad. Let's explore a basic recursive backtracking solution to understand the problem better.

#### Intuition

- Use a map to store each digit's corresponding letters as you would find them on a phone dial.
- For each digit in the input string, iterate over its corresponding letters.
- Use recursive backtracking to build each combination incrementally.
- If a combination has been built corresponding to the length of the digits, add it to the result list.
  
```go
func letterCombinations(digits string) []string {
    if digits == "" {
        return []string{}
    }
    
    // Map of digit to possible corresponding characters
    phoneMap := map[string]string{
        "2": "abc", "3": "def", "4": "ghi", "5": "jkl",
        "6": "mno", "7": "pqrs", "8": "tuv", "9": "wxyz",
    }

    var result []string
    
    // Helper function to generate combinations
    var backtrack func(index int, path string)
    backtrack = func(index int, path string) {
        // Base case: If current combination is the size of digits
        if index == len(digits) {
            result = append(result, path)
            return
        }
        
        // Get the letter corresponding to the current digit
        letters := phoneMap[string(digits[index])]
        for i := 0; i < len(letters); i++ {
            // Append the current letter and move to the next digit
            backtrack(index+1, path+string(letters[i]))
        }
    }

    // Start the backtracking with the first digit
    backtrack(0, "")
    
    return result
}
```

#### Complexity Analysis

- **Time Complexity**: O(3^N * 4^M) where N is the number of digits that maps to 3 letters (e.g. 2, 3, 4, 5, 6, 8) and M is the number of digits that maps to 4 letters (e.g. 7, 9).
- **Space Complexity**: O(3^N * 4^M) to store the combinations in the worst case (all digits).

---

### Iterative with Queue

Instead of recursive backtracking, we can use an iterative approach leveraging a queue to build combinations layer by layer.

#### Intuition

- Start with a single empty string in the queue.
- For each digit, dequeue each partial combination, append each character that the digit can map to, and enqueue the new combinations.
- Keep processing until all digits are accounted for.

```go
func letterCombinations(digits string) []string {
    if digits == "" {
        return []string{}
    }

    // Map of digit to possible corresponding characters
    phoneMap := []string{"abc", "def", "ghi", "jkl", "mno", "pqrs", "tuv", "wxyz"}

    // Initialize the queue with an empty string
    queue := []string{""}

    for i := 0; i < len(digits); i++ {
        // Convert current digit to digit's index for phoneMap
        x := digits[i] - '2'
        length := len(queue)

        // Process each partial combination
        for j := 0; j < length; j++ {
            partial := queue[0]
            queue = queue[1:] // Dequeue operation

            // Append each letter corresponding to the current digit and enqueue
            letters := phoneMap[x]
            for _, ch := range letters {
                queue = append(queue, partial+string(ch))
            }
        }
    }
    
    return queue
}
```

#### Complexity Analysis

- **Time Complexity**: O(3^N * 4^M), as we still must generate all combinations.
- **Space Complexity**: O(3^N * 4^M), storing all combinations.

Using an iterative technique can be beneficial when considering constraints from deep recursion. Both methods are valid and efficient for reasonable input sizes.

