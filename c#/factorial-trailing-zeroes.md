# [Leetcode 172: Factorial Trailing Zeroes](https://leetcode.com/problems/factorial-trailing-zeroes/)

## Approaches
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Count Factors of 5](#approach-2-count-factors-of-5)

## Approach 1: Brute Force

### Intuition
The trailing zeroes in a number are produced by pairs of 2 and 5 in its prime factorization. In a factorial, each even number contributes at least one 2, and multiples of 5 contribute 5. Therefore, every pair of 5 and 2 results in a zero. Given the numbers 2 come more frequently than 5, the number of trailing zeroes is determined by the number of 5s.

In a brute-force approach, one might calculate the factorial of a number and then count the zeroes at the end. However, this is computationally infeasible even for relatively small `n` due to the rapid growth of factorial values.

### Complexity
- Time complexity: O(n log n), approximation of the time taken to multiply numbers and then result in trailing zero counting.
- Space complexity: O(1), aside from input variables.

*Note: This approach is not practical for large values of n due to limitations in handling huge integers in factorial calculations.*

## Approach 2: Count Factors of 5

### Intuition
To find the number of trailing zeroes efficiently, we take advantage of the fact mentioned earlier: trailing zeroes are determined by pairs of factors of 5 and 2. Counting the number of 5s in the factors of numbers from 1 to `n` directly gives us the number of trailing zeroes.

For every multiple of 5 up to `n`, there is at least one 5 in its factorization. Multiples of 25 contribute an additional 5, multiples of 125 contribute another, and so on. Therefore, we consider `n/5 + n/25 + n/125 + ...` to sum up all the contributing factors of 5.

### Algorithm
1. Initialize `count` as 0 to store the number of trailing zeroes.
2. Loop while `n` is greater than 0:
   - Divide `n` by 5 to get how many multiples of 5 contribute at least one 5 to the factorization of numbers up to `n`.
   - Add this contribution to `count`.
3. Return `count`.

### Complexity
- Time complexity: O(log n), because the number of times we divide `n` by 5 is proportional to the number of trailing zeroes.
- Space complexity: O(1), since no additional space is needed other than for input and a few variables.

### Code
```csharp
public class Solution
{
    public int TrailingZeroes(int n)
    {
        // Initialize count to store trailing zeroes
        int count = 0;
        
        // Loop until n becomes zero
        while (n > 0)
        {
            // Reduce n by dividing by 5
            n /= 5;
            // Increase count by the current power of 5 factor count
            count += n;
        }
        
        return count;
    }
}
```

This solution efficiently counts the factors of 5 in numbers from 1 to `n`, effectively calculating trailing zeroes without directly computing factorials, making it suitable for large input sizes.

