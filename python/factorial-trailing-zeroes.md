# [Leetcode Problem 172: Factorial Trailing Zeroes](https://leetcode.com/problems/factorial-trailing-zeroes/)

## Approaches:

1. [Basic Approach: Simulate Factorial and Count Zeroes](#basic-approach)
2. [Optimized Approach: Count Factors of 5](#optimized-approach)

## Basic Approach: Simulate Factorial and Count Zeroes

### Intuition

The trailing zeroes in a factorial are created by the numbers 10, which is the product of 2 and 5. For any factorial, the count of 2s as factors is always more than 5s, so we only need to count the number of 5s in the factors of the numbers from 1 through `n`.

One basic but inefficient way is calculating the factorial of the given number first and then counting the number of trailing zeroes in the resultant number. However, this is not advised for large `n` due to factorial growth and computational limits.

### Python Code

```python
def trailingZeroes(n):
    # Step 1: Calculate factorial
    def factorial(x):
        if x == 0:
            return 1
        else:
            return x * factorial(x - 1)

    # Step 2: Get factorial of n
    fact = factorial(n)

    # Step 3: Count trailing zeroes
    count = 0
    while fact % 10 == 0:
        count += 1
        fact //= 10
    
    return count

# Time Complexity: O(n) for factorial computation
# Space Complexity: O(n) for function call stack used in recursion
```

### Note:
- This approach is not practical for large n due to large factorial values.

## Optimized Approach: Count Factors of 5

### Intuition

To find trailing zeroes, we need to count how many times the factor 5 is present in the numbers from 1 to `n`. This method is optimal as we avoid calculating huge factorial numbers.

Each multiple of 5 contributes at least one factor of 5. Multiples of 25 contribute an extra count, multiples of 125 contribute yet another one, and so on.

### Python Code

```python
def trailingZeroes(n):
    count = 0
    # Step 1: Count factors of 5
    while n >= 5:
        n //= 5
        count += n
    return count

# Time Complexity: O(log5(n)) - The loop runs logarithmically w.r.t n base 5.
# Space Complexity: O(1) - Only a constant amount of space is used.
```

### Explanation:
- Divide `n` by 5, 25, 125, etc., until `n` becomes less than 5.
- Each division quantifies the number of 5-factorial pairs in the counts: `n//5` gives how many multiples of 5 contribute to zeroes, `n//25` for 25s, etc.

This method efficiently solves the problem without directly computing the factorial, making it feasible for large inputs.

