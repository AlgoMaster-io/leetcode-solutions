# [Leetcode 17: Letter Combinations of a Phone Number](https://leetcode.com/problems/letter-combinations-of-a-phone-number/)

## Approaches
1. [Backtracking with Recursion](#approach-1-backtracking-with-recursion)
2. [Iterative Approach using a Queue](#approach-2-iterative-approach-using-a-queue)

## Approach 1: Backtracking with Recursion

The problem can be broken down into generating combinations of letters based on the mapping of digits to letters like on a traditional phone keypad. Each digit can correspond to multiple possible letters, and the challenge is to create all possible letter combinations that the phone number could represent.

### Intuition

Backtracking is ideal for generating all possible combinations as it allows us to systematically explore all potential candidates, expanding and contracting the solution path as necessary. It's a trial-and-error method which involves building combinations incrementally and throwing away partial combinations as soon as it determines that they cannot successfully lead to a complete solution.

### Steps

1. **Mapping Digits to Letters**: Use a dictionary to map each digit to its corresponding possible letters.
2. **Backtrack Function**: Implement a recursive function that takes the current combination of letters and the next digits to process.
3. **Complete Combination**: If there are no more digits to check, add the current combination to the result list.
4. **Recursive Exploration**: For every letter in the current digit's mapping, append it to the current combination and proceed recursively with the remaining digits.

### Code

```python
def letterCombinations(digits: str):
    if not digits:
        return []

    phone_map = {
        '2': 'abc', '3': 'def', '4': 'ghi', '5': 'jkl',
        '6': 'mno', '7': 'pqrs', '8': 'tuv', '9': 'wxyz'
    }
    
    def backtrack(index: int, path: str):
        # If the path we've built has the same length as digits input
        if index == len(digits):
            combinations.append(path)
            return
        
        # Get the letters that the current digit maps to
        possible_letters = phone_map[digits[index]]
        
        # Explore all possibilities with the current digit
        for letter in possible_letters:
            backtrack(index + 1, path + letter)
    
    combinations = []
    backtrack(0, "")
    return combinations
```

### Complexity Analysis

- **Time Complexity**: O(3^N * 4^M), where N is the number of digits that can map to 3 letters ('2', '3', '4', '5', '6', '8') and M is the number of digits that can map to 4 letters ('7', '9').
- **Space Complexity**: O(3^N * 4^M), which is needed for storing the results of the combinations.

---

## Approach 2: Iterative Approach using a Queue

In the iterative approach, we can use a queue (typically implemented using a list in Python) to build combinations step by step.

### Intuition

This approach allows us to gradually build up combinations by processing each digit and expanding the existing partial combinations until we have the full set. The idea is to start with an initial empty combination and iteratively append all valid letters that correspond to each digit.

### Steps

1. **Initialize Queue**: Start with a list containing an empty string to build upon.
2. **Iterative Processing**: For each digit, process current combinations, and append corresponding letters.
3. **Complete Combination**: Every time we fully process a digit by adding its letters, we move onto the next digit’s letters.

### Code

```python
from collections import deque

def letterCombinationsIterative(digits: str):
    if not digits:
        return []

    phone_map = {
        '2': 'abc', '3': 'def', '4': 'ghi', '5': 'jkl',
        '6': 'mno', '7': 'pqrs', '8': 'tuv', '9': 'wxyz'
    }
    
    # Use a queue to manage the building process of combinations
    queue = deque([''])
    
    for digit in digits:
        possible_letters = phone_map[digit]
        # For each letter in the current digit's possible letters
        for _ in range(len(queue)):
            # pop combinations that need to be extended
            combination = queue.popleft()
            for letter in possible_letters:
                queue.append(combination + letter)
    
    return list(queue)
```

### Complexity Analysis

- **Time Complexity**: O(3^N * 4^M), similar to the recursive approach as each combination can still be produced with these steps.
- **Space Complexity**: O(3^N * 4^M), used for storing all combinations in the queue.

By understanding both recursive backtracking and iterative queue-based approaches, one can solve the problem of generating combinations of phone number letters effectively, exploring and mastering the methods of both recursive and iterative problem solving in algorithm design.

