# 686. [Repeated String Match](https://leetcode.com/problems/repeated-string-match/)

## Approach 1: Brute Force

### Solution
```cpp
// Time Complexity: O(n * m), where n is the length of A and m is the length of B
// Space Complexity: O(n + m)
#include <string>

class Solution {
public:
    int repeatedStringMatch(const std::string &A, const std::string &B) {
        std::string repeatedA = A;
        int repeatCount = 1;

        // Repeat A until its length is at least the length of B
        while (repeatedA.length() < B.length()) {
            repeatedA += A;
            repeatCount++;
        }

        // Check if B is a substring of the repeated A
        if (repeatedA.find(B) != std::string::npos) {
            return repeatCount;
        }

        // Add one more repetition just in case B starts towards the end
        repeatedA += A;
        if (repeatedA.find(B) != std::string::npos) {
            return repeatCount + 1;
        }

        return -1; // B is not a substring of repeated A
    }
};
```

## Approach 2: Optimized Approach using Rolling Window

### Solution
```cpp
// Time Complexity: O(n + m), where n is the length of A and m is the length of B
// Space Complexity: O(n + m)
#include <string>

class Solution {
public:
    int repeatedStringMatch(const std::string &A, const std::string &B) {
        int m = A.length();
        int n = B.length();
        int maxRepeat = (n / m) + 2; // Only need to repeat at most this many times

        std::string repeatedA;
        for (int i = 0; i < maxRepeat; i++) {
            repeatedA += A;
            // Check if B is a substring of the current repeated A
            if (repeatedA.find(B) != std::string::npos) {
                return i + 1;
            }
        }

        return -1; // B is not a substring of repeated A
    }
};
```

