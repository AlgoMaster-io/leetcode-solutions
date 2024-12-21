# [Leetcode 172: Factorial Trailing Zeroes](https://leetcode.com/problems/factorial-trailing-zeroes/)

## Table of Contents
1. [Basic Approach: Count Factors of 5](#basic-approach)
2. [Optimized Approach: Mathematical Insight](#optimized-approach)

### Problem Overview

The problem is to find the number of trailing zeroes in the factorial of a given number `n`. A trailing zero is created by a pair of factors 2 and 5. Therefore, to count the number of trailing zeroes, we need to count how many pairs of 2 and 5 can be formed from the numbers in the factorial decomposition. Since in most cases, there are more 2s than 5s, the task reduces to counting the number of 5s in the prime factors of numbers from 1 to `n`.

## [Approach 1: Count Factors of 5](#basic-approach)

### Intuition
The basic idea is to count how many times 5 is a factor of numbers from 1 to `n`. Each multiple of 5 contributes at least one 5 to the factorization. For example, numbers like 25, 125, etc., which throw an additional power of 5, need to be considered as well.

### Steps
1. Iterate from 5 to `n`, stepping by 5s.
2. Count how many times 5 is a factor.
3. Return the count as the number of trailing zeroes.

### Code
```java
public class Solution {
    public int trailingZeroes(int n) {
        // Initialize the count of trailing zeroes
        int count = 0;
        
        // Count factors of 5
        for (int i = 5; i <= n; i *= 5) {
            // Add the multiples of 5
            count += n / i;
        }
        
        return count;
    }
}
```

### Time Complexity
- **O(log5(n))**: We are dividing `n` by powers of 5.

### Space Complexity
- **O(1)**: Constant space is used.

## [Approach 2: Optimized Approach: Mathematical Insight](#optimized-approach)

### Intuition
The count of trailing zeroes is determined by the number of times 5 is a factor in the numbers from 1 to `n`. The mathematical insight is that we only count multiples of 5, 25, 125, etc., because they contribute one or more extra 5s. 

### Steps
1. Calculate how many multiples of 5, 25, 125, etc., there are in numbers up to `n`.
2. This can be done by continuously dividing `n` by 5, 25, 125... (i.e., continuously dividing by 5) and summing the quotient.
3. The sum will be our result.

### Code
```java
public class Solution {
    public int trailingZeroes(int n) {
        // Initialize the count of trailing zeroes
        int count = 0;
        
        // Repeatedly divide n by 5 and sum the whole number quotients
        while (n >= 5) {
            n /= 5;
            count += n;
        }
        
        return count;
    }
}
```

### Time Complexity
- **O(log5(n))**: Each division step reduces `n` by a factor of 5.

### Space Complexity
- **O(1)**: We are using constant space.

