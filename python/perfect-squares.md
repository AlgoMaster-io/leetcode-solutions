# [Leetcode 279: Perfect Squares](https://leetcode.com/problems/perfect-squares/)

## Approaches

1. [Brute Force with Recursion](#approach-1)
2. [Dynamic Programming](#approach-2)
3. [Breadth-First Search (BFS)](#approach-3)
4. [Mathematical Approach (Lagrange's Four-Square Theorem)](#approach-4)

### Approach 1: Brute Force with Recursion

The most straightforward approach is the brute force method using recursion, where we attempt to subtract perfect squares repeatedly until we reach zero.

#### Intuition

For any number `n`, try subtracting squares of numbers `(k*k)` and recursively calculate the minimum number of perfect squares for `n - k*k`.

Given the recursive nature, the solution breaks down into smaller subproblems which can be repetitive, leading to an exponential time complexity.

#### Code

```python
def numSquares(n):
    def minSquares(k):
        # Base case: if k is 0, no square is needed
        if k == 0:
            return 0
        
        min_count = float('inf')
        
        # Try every square number less than or equal to k
        i = 1
        while i * i <= k:
            # Recursively find the minimum for the remaining k
            min_count = min(min_count, 1 + minSquares(k - i * i))
            i += 1
            
        return min_count
    
    return minSquares(n)

# Time Complexity: O(n^n) in the worst case due to the repetitive calculations.
# Space Complexity: O(n) considering the depth of recursion.
```

### Approach 2: Dynamic Programming

#### Intuition

Dynamic Programming can drastically reduce the number of redundant calculations. We use a DP array where `dp[i]` stores the least number of perfect squares that sum to `i`.

For each number, compute the minimum from previous results plus one (for the current square considered).

#### Code

```python
def numSquares(n):
    dp = [float('inf')] * (n + 1)
    dp[0] = 0  # Base case: 0 needs 0 perfect squares

    for i in range(1, n + 1):
        j = 1
        while j * j <= i:
            # Consider the current square number `j*j` and update dp[i]
            dp[i] = min(dp[i], dp[i - j * j] + 1)
            j += 1

    return dp[n]

# Time Complexity: O(n * sqrt(n)), as for each number up to n, it may calculations up to sqrt(n) squares.
# Space Complexity: O(n), due to storage of results in the dp array.
```

### Approach 3: Breadth-First Search (BFS)

#### Intuition

This approach models the problem as a graph traversal problem where each node represents a number `i` and an edge exists between nodes `i` and `i - j*j`.

Perform BFS to find the shortest path to zero, guaranteeing the minimum number of steps (i.e., minimum number of perfect squares).

#### Code

```python
from collections import deque

def numSquares(n):
    queue = deque([(n, 0)])  # Queue of (current_number, step_count)
    visited = set()

    while queue:
        number, step = queue.popleft()
        
        if number == 0:
            return step  # When we reach zero, return the step count as the answer
        
        # Check all possible squares we can subtract
        i = 1
        while i * i <= number:
            new_number = number - i * i
            if new_number not in visited:
                queue.append((new_number, step + 1))
                visited.add(new_number)
            i += 1

# Time Complexity: O(n), since each number is visited once in BFS.
# Space Complexity: O(n), considering the space needed for the queue and visited set.
```

### Approach 4: Mathematical Approach (Lagrange's Four-Square Theorem)

#### Intuition

According to Lagrange's Four-Square Theorem, any positive integer can be represented as the sum of four integer squares. For `n`, check if it can be made with 1, 2, or 3 squares directly. If not, it will definitely be 4 due to the theorem.

#### Code

```python
import math

def isPerfectSquare(x):
    s = int(math.sqrt(x))
    return s * s == x

def numSquares(n):
    if isPerfectSquare(n):
        return 1
    
    # Reduce n by removing factors of 4
    while n % 4 == 0:
        n //= 4
    
    # Check if n after reduction is in form 4^a*(8b + 7)
    if n % 8 == 7:
        return 4
    
    # Check if result is 2
    for i in range(1, int(n**0.5) + 1):
        if isPerfectSquare(n - i*i):
            return 2
    
    return 3

# Time Complexity: O(sqrt(n)), for checking perfect squares.
# Space Complexity: O(1), as no extra space other than variables are used.
```

Each of these methods provides a different lens to tackle the problem of finding the least number of perfect squares that sum up to a given integer `n`. The mathematical approach often provides the fastest results due to leveraging number theory.

