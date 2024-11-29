# 686. [Repeated String Match](https://leetcode.com/problems/repeated-string-match/)

## Approach 1: Brute Force

### Solution
csharp
```csharp
// Time Complexity: O(n * m), where n is the length of A and m is the length of B
// Space Complexity: O(n + m)
public class Solution {
    public int RepeatedStringMatch(string A, string B) {
        StringBuilder repeatedA = new StringBuilder(A);
        int repeatCount = 1;

        // Repeat A until its length is at least the length of B
        while (repeatedA.Length < B.Length) {
            repeatedA.Append(A);
            repeatCount++;
        }

        // Check if B is a substring of the repeated A
        if (repeatedA.ToString().Contains(B)) {
            return repeatCount;
        }

        // Add one more repetition just in case B starts towards the end
        repeatedA.Append(A);
        if (repeatedA.ToString().Contains(B)) {
            return repeatCount + 1;
        }

        return -1; // B is not a substring of repeated A
    }
}
```

## Approach 2: Optimized Approach using Rolling Window

### Solution
csharp
```csharp
// Time Complexity: O(n + m), where n is the length of A and m is the length of B
// Space Complexity: O(n + m)
public class Solution {
    public int RepeatedStringMatch(string A, string B) {
        int m = A.Length;
        int n = B.Length;
        int maxRepeat = (n / m) + 2; // Only need to repeat at most this many times

        StringBuilder repeatedA = new StringBuilder();
        for (int i = 0; i < maxRepeat; i++) {
            repeatedA.Append(A);
            // Check if B is a substring of the current repeated A
            if (repeatedA.ToString().IndexOf(B) != -1) {
                return i + 1;
            }
        }

        return -1; // B is not a substring of repeated A
    }
}
```

