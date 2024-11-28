# [202. Happy Number](https://leetcode.com/problems/happy-number/)

## Approach 1: Using HashSet to Detect Cycles

### Solution
```java
// Time Complexity: O(log(n))
// Space Complexity: O(log(n))
import java.util.HashSet;

public class Solution {
    public boolean isHappy(int n) {
        HashSet<Integer> seen = new HashSet<>();

        while (n != 1 && !seen.contains(n)) {
            seen.add(n);
            n = getNextNumber(n);
        }

        return n == 1;
    }

    private int getNextNumber(int n) {
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
```java
// Time Complexity: O(log(n))
// Space Complexity: O(1)
public class Solution {
    public boolean isHappy(int n) {
        int slow = n;
        int fast = getNextNumber(n);

        while (fast != 1 && slow != fast) {
            slow = getNextNumber(slow);
            fast = getNextNumber(getNextNumber(fast));
        }

        return fast == 1;
    }

    private int getNextNumber(int n) {
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