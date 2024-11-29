# 50. [Pow(x, n)](https://leetcode.com/problems/powx-n/)

## Approach 1: Iterative with Binary Exponentiation

### Solution
java
```java
// Time Complexity: O(log n)
// Space Complexity: O(1)
public class Solution {
    public double myPow(double x, int n) {
        if (n == 0) return 1;
        long N = n;
        if (N < 0) {
            x = 1 / x;
            N = -N;
        }
        double result = 1;
        while (N > 0) {
            if ((N & 1) == 1) {
                result *= x;
            }
            x *= x;
            N >>= 1;
        }
        return result;
    }
}
```

go
```go
// Time Complexity: O(log n)
// Space Complexity: O(1)
func myPow(x float64, n int) float64 {
    if n == 0 {
        return 1
    }
    N := int64(n)
    if N < 0 {
        x = 1 / x
        N = -N
    }
    result := 1.0
    for N > 0 {
        if N&1 == 1 {
            result *= x
        }
        x *= x
        N >>= 1
    }
    return result
}
```

## Approach 2: Recursive with Binary Exponentiation

### Solution
java
```java
// Time Complexity: O(log n)
// Space Complexity: O(log n)
public class Solution {
    public double myPow(double x, int n) {
        if (n < 0) {
            x = 1 / x;
            n = -n;
        }
        return fastPow(x, n);
    }

    private double fastPow(double x, int n) {
        if (n == 0) {
            return 1.0;
        }
        double half = fastPow(x, n / 2);
        if (n % 2 == 0) {
            return half * half;
        } else {
            return half * half * x;
        }
    }
}
```

go
```go
// Time Complexity: O(log n)
// Space Complexity: O(log n)
func myPow(x float64, n int) float64 {
    if n < 0 {
        x = 1 / x
        n = -n
    }
    return fastPow(x, n)
}

func fastPow(x float64, n int) float64 {
    if n == 0 {
        return 1.0
    }
    half := fastPow(x, n / 2)
    if n % 2 == 0 {
        return half * half
    } else {
        return half * half * x
    }
}
```



