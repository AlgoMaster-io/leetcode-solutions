# [Leetcode 357: Count Numbers with Unique Digits](https://leetcode.com/problems/count-numbers-with-unique-digits/)

This problem requires us to count numbers with unique digits given a non-negative integer `n` representing the number of digits. Let's explore different approaches to solve this problem.

## Approaches
- [Brute Force Approach](#brute-force-approach)
- [Mathematical Combinatorics Approach](#mathematical-combinatorics-approach)

### Brute Force Approach
In a brute force approach, we can generate all numbers with up to `n` digits and count those with unique digits. However, this method will be computationally expensive due to the vast number of possibilities, making it less suitable for larger values of `n`.

#### Intuition:
1. Generate all numbers with up to `n` digits.
2. Check each number to see if it contains unique digits.
3. Count those numbers.

```java
public class Solution {
    public int countNumbersWithUniqueDigits(int n) {
        if (n == 0) return 1; // There's only one number, 0, when n is 0.
        
        int count = 1; // Start with 0 being a unique number.
        for (int i = 1; i <= Math.pow(10, n) - 1; i++) {
            if (hasUniqueDigits(i)) {
                count++;
            }
        }
        return count;
    }
    
    private boolean hasUniqueDigits(int num) {
        boolean[] seen = new boolean[10]; // Array to keep track of seen digits.
        while (num > 0) {
            int digit = num % 10; // Extract the last digit.
            if (seen[digit]) return false; // If it's already seen, return false.
            seen[digit] = true; // Mark the digit as seen.
            num /= 10; // Move to the next digit.
        }
        return true;
    }
}
```

**Time Complexity**: O(10^n * d) where `d` is the number of digits, as we have to check each number up to `10^n`.  
**Space Complexity**: O(1), aside from the input and a boolean array used for verifying digit uniqueness.

### Mathematical Combinatorics Approach

Instead of generating each number, we can use combinatorial counting to determine how many numbers have unique digits. 

#### Intuition:
- For a given `n`:
  - The first digit has 9 options (1 to 9), as 0 cannot be the first digit.
  - The second digit has 9 options (0 to 9 except the first digit).
  - The third digit has 8 options, and so on.
- Use combinatorial multiplication to compute the count of numbers with unique digits.

```java
public class Solution {
    public int countNumbersWithUniqueDigits(int n) {
        if (n == 0) return 1; // Base case, only the number 0.

        int result = 10; // For n = 1, there are 10 single-digit numbers: 0 through 9
        int uniqueDigits = 9; // First digit options for n > 1 (1-9)
        int availableNumbers = 9; // Remaining digit options (0-9 except used ones)

        while (n-- > 1 && availableNumbers > 0) {
            uniqueDigits *= availableNumbers;
            result += uniqueDigits;
            availableNumbers--;
        }
        
        return result;
    }
}
```

**Time Complexity**: O(n), as each digit added requires constant time calculations.  
**Space Complexity**: O(1), as we are not using any additional space that grows with `n`.

These approaches demonstrate trade-offs between direct computation and combinatorial analytical optimization. For large values of `n`, the mathematical approach is significantly more efficient.

