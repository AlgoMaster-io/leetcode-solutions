# 357. [Count Numbers with Unique Digits](https://leetcode.com/problems/count-numbers-with-unique-digits/)

## Approach 1: Recursive Backtracking

### Solution

```csharp
// Time Complexity: O(10^n)
// Space Complexity: O(n)
public class Solution {
    public int CountNumbersWithUniqueDigits(int n) {
        if (n == 0) return 1;
        bool[] used = new bool[10];
        return CountNumbers(0, n, used);
    }

    private int CountNumbers(int current, int n, bool[] used) {
        if (n == 0) return 1;
        int count = 1;  // count the number itself
        for (int i = 0; i < 10; i++) {
            if (!used[i]) {
                used[i] = true;
                count += CountNumbers(current * 10 + i, n - 1, used);
                used[i] = false;
            }
        }
        return count;
    }
}
```

## Approach 2: Dynamic Programming

### Solution

```csharp
// Time Complexity: O(n)
// Space Complexity: O(1)
public class Solution {
    public int CountNumbersWithUniqueDigits(int n) {
        if (n == 0) return 1;

        int uniqueNumbers = 10, availableDigits = 9, currentUnique = 9;

        // Use dynamic programming to calculate the number
        for (int i = 2; i <= n; i++) {
            currentUnique *= availableDigits;
            uniqueNumbers += currentUnique;
            availableDigits--;
        }

        return uniqueNumbers;
    }
}
```

## Approach 3: Formula-based Calculation

### Solution

```csharp
// Time Complexity: O(n)
// Space Complexity: O(1)
public class Solution {
    public int CountNumbersWithUniqueDigits(int n) {
        if (n == 0) return 1;

        int count = 10;  // start counting from single-digit numbers
        int uniqueDigits = 9, availableNumbers = 9;

        // Calculate using the permutation formula for unique digits
        while (n > 1 && availableNumbers > 0) {
            uniqueDigits *= availableNumbers;
            count += uniqueDigits;
            availableNumbers--;
            n--;
        }

        return count;
    }
}
```

