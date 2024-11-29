# 686. [Repeated String Match](https://leetcode.com/problems/repeated-string-match/)

## Approach 1: Brute Force

### Solution
```java
// Time Complexity: O(n * m), where n is the length of A and m is the length of B
// Space Complexity: O(n + m)
public class Solution {
    public int repeatedStringMatch(String A, String B) {
        StringBuilder repeatedA = new StringBuilder(A);
        int repeatCount = 1;

        // Repeat A until it's length is at least length of B
        while (repeatedA.length() < B.length()) {
            repeatedA.append(A);
            repeatCount++;
        }

        // Check if B is a substring of the repeated A
        if (repeatedA.toString().contains(B)) {
            return repeatCount;
        }

        // Add one more repetition just in case B starts towards the end
        repeatedA.append(A);
        if (repeatedA.toString().contains(B)) {
            return repeatCount + 1;
        }

        return -1; // B is not a substring of repeated A
    }
}
```

## Approach 2: Optimized Approach using Rolling Window

### Solution
```java
// Time Complexity: O(n + m), where n is the length of A and m is the length of B
// Space Complexity: O(n + m)
public class Solution {
    public int repeatedStringMatch(String A, String B) {
        int m = A.length();
        int n = B.length();
        int maxRepeat = (n / m) + 2; // Only need to repeat at most this many times

        StringBuilder repeatedA = new StringBuilder();
        for (int i = 0; i < maxRepeat; i++) {
            repeatedA.append(A);
            // Check if B is a substring of the current repeated A
            if (repeatedA.toString().indexOf(B) != -1) {
                return i + 1;
            }
        }

        return -1; // B is not a substring of repeated A
    }
}
```

