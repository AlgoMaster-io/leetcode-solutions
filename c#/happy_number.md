# [202. Happy Number](https://leetcode.com/problems/happy-number/)

## Approach 1: Using HashSet to Detect Cycles

### Solution
csharp
```csharp
// Time Complexity: O(log(n))
// Space Complexity: O(log(n))
using System.Collections.Generic;

public class Solution {
    public bool IsHappy(int n) {
        HashSet<int> seen = new HashSet<int>();

        while (n != 1 && !seen.Contains(n)) {
            seen.Add(n);
            n = GetNextNumber(n);
        }

        return n == 1;
    }

    private int GetNextNumber(int n) {
        int sum = 0;

        while (n > 0) {
            int digit = n % 10;
            sum += digit * digit;
            n /= 10;
        }

        return sum;
    }
}
```

## Approach 2: Fast and Slow Pointers (Optimal for Cycle Detection)

### Solution
csharp
```csharp
// Time Complexity: O(log(n))
// Space Complexity: O(1)
public class Solution {
    public bool IsHappy(int n) {
        int slow = n;
        int fast = GetNextNumber(n);

        while (fast != 1 && slow != fast) {
            slow = GetNextNumber(slow);
            fast = GetNextNumber(GetNextNumber(fast));
        }

        return fast == 1;
    }

    private int GetNextNumber(int n) {
        int sum = 0;

        while (n > 0) {
            int digit = n % 10;
            sum += digit * digit;
            n /= 10;
        }

        return sum;
    }
}
```

