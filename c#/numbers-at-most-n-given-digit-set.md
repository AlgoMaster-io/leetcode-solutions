[Leetcode 902: Numbers At Most N Given Digit Set](https://leetcode.com/problems/numbers-at-most-n-given-digit-set/)

## Approaches
- [Approach 1: Backtracking](#approach-1-backtracking)
- [Approach 2: Dynamic Programming](#approach-2-dynamic-programming)

---

### Approach 1: Backtracking

**Intuition:**

The problem asks us to count how many numbers, composed of a given digit set, are less than or equal to a number `N`. A straightforward approach is to generate all possible numbers using backtracking and check their validity. 

We start by considering numbers of increasing length and check if we can construct them using the given digits and if they are valid (i.e., less than or equal to `N`).

**Steps:**

1. Convert `N` to a string to easily compare numbers of the same length.
2. Use backtracking to form numbers starting from an empty string.
3. For each digit in our digit set:
   - Append it to the current string if the result is still valid.
   - Recur until the number reaches the length of `N`.
   - If the formed number is less than or equal to `N`, count it.

**Code:**

```csharp
public class Solution {
    public int AtMostNGivenDigitSet(string[] digits, int N) {
        string nStr = N.ToString();
        int count = 0;

        bool Backtrack(string current) {
            // Base case: If length of current string matches length of nStr
            if (current.Length == nStr.Length) {
                if (string.Compare(current, nStr) <= 0) {
                    count++;
                }
                return current.Length > nStr.Length;
            }

            foreach (string digit in digits) {
                // Try adding the digit and backtrack
                if (current.Length < nStr.Length) {
                    if (Backtrack(current + digit)) break;
                }
            }

            return false;
        }

        Backtrack("");
        return count;
    }
}
```

**Time Complexity:** O(d^l) where d is the number of digits in the digit set and l is the length of the number `N`.
**Space Complexity:** O(l) for the recursion stack.

---

### Approach 2: Dynamic Programming

**Intuition:**

To enhance efficiency, we can use dynamic programming by breaking down the problem and calculating the number of valid numbers by determining the number of possible combinations for each length and prefix incrementally:

1. Count numbers with lengths less than `N`.
2. For numbers with the same length as `N`, construct them digit by digit while checking with `N` to ensure they do not exceed it.

**Steps:**

1. Calculate the total numbers we can make with lengths less than `N`.
2. Calculate numbers with length equal to `N`:
   - Attempt to construct `N` from left to right using digits.
   - Use a dynamic programming array or similar technique to store result increments.

**Code:**

```csharp
public class Solution {
    public int AtMostNGivenDigitSet(string[] digits, int N) {
        string nStr = N.ToString();
        int length = nStr.Length;
        int dsize = digits.Length;

        // Count numbers with lengths less than N
        int totalCount = 0;
        for (int i = 1; i < length; i++) {
            totalCount += (int)Math.Pow(dsize, i);
        }

        // Count numbers with the same length as N
        for (int i = 0; i < length; i++) {
            bool hasSameDigit = false;
            foreach (string digit in digits) {
                if (digit[0] < nStr[i]) {
                    totalCount += (int)Math.Pow(dsize, length - i - 1);
                } else if (digit[0] == nStr[i]) {
                    hasSameDigit = true;
                }
            }
            if (!hasSameDigit) return totalCount;
        }
        return totalCount + 1;
    }
}
```

**Time Complexity:** O(l * d), where l is the length of N and d is the number of digits in the digit set.
**Space Complexity:** O(1), as no additional data structures that grow with input size are used.

