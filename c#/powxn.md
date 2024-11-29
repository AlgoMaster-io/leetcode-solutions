# 50. [Pow(x, n)](https://leetcode.com/problems/powx-n/)

## Approach 1: Recursive Fast Exponentiation

### Solution
java
```java
// Time Complexity: O(log n)
// Space Complexity: O(log n) due to the recursion stack
class Solution {
    public double myPow(double x, int n) {
        if (n == 0) return 1.0;
        if (n < 0) {
            x = 1 / x;
            n = -n;
        }
        return n % 2 == 0 ? myPow(x * x, n / 2) : x * myPow(x * x, n / 2);
    }
}
```

c#
```csharp
// Time Complexity: O(log n)
// Space Complexity: O(log n) due to the recursion stack
public class Solution {
    public double MyPow(double x, int n) {
        if (n == 0) return 1.0;
        if (n < 0) {
            x = 1 / x;
            n = -n;
        }
        return n % 2 == 0 ? MyPow(x * x, n / 2) : x * MyPow(x * x, n / 2);
    }
}
```

## Approach 2: Iterative Fast Exponentiation

### Solution
java
```java
// Time Complexity: O(log n)
// Space Complexity: O(1)
class Solution {
    public double myPow(double x, int n) {
        if (n == 0) return 1.0;
        long N = n; // Use long to handle Integer.MIN_VALUE
        if (N < 0) {
            x = 1 / x;
            N = -N;
        }
        double result = 1.0;
        while (N > 0) {
            if (N % 2 == 1) { // If N is odd
                result *= x;
            }
            x *= x;
            N /= 2;
        }
        return result;
    }
}
```

c#
```csharp
// Time Complexity: O(log n)
// Space Complexity: O(1)
public class Solution {
    public double MyPow(double x, int n) {
        if (n == 0) return 1.0;
        long N = n; // Use long to handle Integer.MIN_VALUE
        if (N < 0) {
            x = 1 / x;
            N = -N;
        }
        double result = 1.0;
        while (N > 0) {
            if (N % 2 == 1) { // If N is odd
                result *= x;
            }
            x *= x;
            N /= 2;
        }
        return result;
    }
}
```

