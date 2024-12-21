# [Leetcode 72: Edit Distance](https://leetcode.com/problems/edit-distance/)

## Approaches:
1. [Recursive Approach](#recursive-approach)
2. [Recursive with Memoization](#recursive-with-memoization)
3. [Dynamic Programming Approach](#dynamic-programming-approach)
4. [Dynamic Programming with Space Optimization](#dynamic-programming-with-space-optimization)

## Recursive Approach

### Intuition:
The edit distance problem is essentially finding the minimum number of operations required to transform one string into another. The possible operations are insertion, deletion, and substitution. Using a recursive approach, we can compare the last characters of both strings and decide on the minimum operation needed. By recursively solving subproblems, we can handle all possible transformations.

### Detailed Steps:
- Compare the last characters of the two strings.
- If they are the same, move both pointers backward.
- If they are different, consider all operations:
  - Insert a character.
  - Delete a character.
  - Substitute a character.
- Recur for all three operations and choose the one with the minimal cost.

### Code:
```python
def minDistanceRecursive(word1, word2):
    def helper(i, j):
        # Base cases: if one of the strings is empty
        if i == 0:
            return j
        if j == 0:
            return i
        # If characters are the same, no operation needed
        if word1[i-1] == word2[j-1]:
            return helper(i-1, j-1)
        # If characters are different, consider all operations
        insert_op = helper(i, j-1)  # insert
        delete_op = helper(i-1, j)  # delete
        replace_op = helper(i-1, j-1)  # replace
        return 1 + min(insert_op, delete_op, replace_op)
    
    return helper(len(word1), len(word2))

# Time Complexity: O(3^(m+n)) - exponential time complexity 
# Space Complexity: O(m+n) - stack space for recursion
```

## Recursive with Memoization

### Intuition:
The recursive approach recalculates the same subproblems multiple times. By storing the results of already solved subproblems using memoization, we can significantly reduce the time complexity.

### Detailed Steps:
- Similar to the recursive approach, but create a dictionary to store the results of subproblems.
- Before calculating the edit distance for a pair of indices, check the dictionary to see if it has already been solved.

### Code:
```python
def minDistanceRecursiveMemo(word1, word2):
    memo = {}
    
    def helper(i, j):
        if (i, j) in memo:
            return memo[(i, j)]
        if i == 0:
            return j  # need j insertions
        if j == 0:
            return i  # need i deletions
        if word1[i-1] == word2[j-1]:
            memo[(i, j)] = helper(i-1, j-1)
        else:
            insert_op = helper(i, j-1)
            delete_op = helper(i-1, j)
            replace_op = helper(i-1, j-1)
            memo[(i, j)] = 1 + min(insert_op, delete_op, replace_op)
        return memo[(i, j)]
    
    return helper(len(word1), len(word2))

# Time Complexity: O(m * n) - where m and n are the lengths of the strings
# Space Complexity: O(m * n) - for memoization storage
```

## Dynamic Programming Approach

### Intuition:
The dynamic programming approach uses a 2D array where each element `dp[i][j]` represents the minimum edit distance for the strings `word1[:i]` and `word2[:j]`. This avoids recalculations by using previously computed values.

### Detailed Steps:
1. Initialize a 2D array `dp` with dimensions `(m+1) x (n+1)`.
2. Base Case Initialization: 
   - Transforming an empty string to `word2` requires `j` insertions.
   - Transforming `word1` to an empty string requires `i` deletions.
3. Fill the array based on previously discussed recursive relation over all `i` and `j`.
4. The value at `dp[m][n]` gives the edit distance for the entire strings.

### Code:
```python
def minDistanceDP(word1, word2):
    m, n = len(word1), len(word2)
    dp = [[0] * (n + 1) for _ in range(m + 1)]
    
    # Initialize base cases
    for i in range(m + 1):
        dp[i][0] = i  # delete all characters in word1
    
    for j in range(n + 1):
        dp[0][j] = j  # insert all characters in word2
    
    # Fill the table
    for i in range(1, m + 1):
        for j in range(1, n + 1):
            if word1[i - 1] == word2[j - 1]:
                dp[i][j] = dp[i - 1][j - 1]  # No change needed
            else:
                insert_op = dp[i][j - 1]
                delete_op = dp[i - 1][j]
                replace_op = dp[i - 1][j - 1]
                dp[i][j] = 1 + min(insert_op, delete_op, replace_op)
    
    return dp[m][n]

# Time Complexity: O(m * n) - double iteration over the strings 
# Space Complexity: O(m * n) - 2D dp array usage
```

## Dynamic Programming with Space Optimization

### Intuition:
Since the current row in the DP table is dependent only on the previous row, we can reduce the space complexity to `O(n)` by keeping track of only two rows: current and previous.

### Detailed Steps:
1. Use two arrays, `previous` and `current`, to keep track of the edit distances.
2. At each iteration, update the `current` based on `previous` and reset after each row.
3. Finally, return the last element which represents the edit distance.

### Code:
```python
def minDistanceDPOptimized(word1, word2):
    m, n = len(word1), len(word2)
    
    # Initialize two rows for the current and previous computations
    previous = [0] * (n + 1)
    current = [0] * (n + 1)
    
    # Initializing the base case
    for j in range(n + 1):
        previous[j] = j
    
    # Fill the array
    for i in range(1, m + 1):
        current[0] = i  # Base case: transforming word1 to empty word2
        for j in range(1, n + 1):
            if word1[i - 1] == word2[j - 1]:
                current[j] = previous[j - 1]
            else:
                insert_op = current[j - 1]
                delete_op = previous[j]
                replace_op = previous[j - 1]
                current[j] = 1 + min(insert_op, delete_op, replace_op)
        previous, current = current, previous
    
    return previous[n]

# Time Complexity: O(m * n) - double iteration over the length of the strings
# Space Complexity: O(n) - We use two arrays to store intermediate results
```

Choose the approach that best fits your constraints based on time and space trade-offs. The space-optimized dynamic programming solution is typically the most efficient in practice.

