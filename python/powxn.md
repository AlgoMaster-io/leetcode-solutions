# 50. [Pow(x, n)](https://leetcode.com/problems/powx-n/)

## Approach 1: Recursive Fast Power

### Solution
java
```java
// Time Complexity: O(log n)
// Space Complexity: O(log n)
public class Solution {
    public double myPow(double x, int n) {
        long longN = n; // Use long to prevent overflow issues
        return longN >= 0 ? fastPow(x, longN) : 1.0 / fastPow(x, -longN);
    }
    
    private double fastPow(double x, long n) {
        if (n == 0) {
            return 1.0;
        }
        
        double half = fastPow(x, n / 2);
        return n % 2 == 0 ? half * half : half * half * x;
    }
}
```

python
```python
# Time Complexity: O(log n)
# Space Complexity: O(log n)
class Solution:
    def myPow(self, x: float, n: int) -> float:
        def fast_pow(x, n):
            if n == 0:
                return 1.0
    
            half = fast_pow(x, n // 2)
            return half * half if n % 2 == 0 else half * half * x
    
        long_n = n
        return fast_pow(x, long_n) if long_n >= 0 else 1.0 / fast_pow(x, -long_n)
```

## Approach 2: Iterative Fast Power

### Solution
java
```java
// Time Complexity: O(log n)
// Space Complexity: O(1)
public class Solution {
    public double myPow(double x, int n) {
        long longN = n; // Use long to prevent overflow issues
        double result = 1.0;
        double currentProduct = x;

        long absN = Math.abs(longN);
        while (absN > 0) {
            if ((absN % 2) == 1) {
                result *= currentProduct;
            }
            currentProduct *= currentProduct;
            absN /= 2;
        }

        return longN < 0 ? 1.0 / result : result;
    }
}
```

python
```python
# Time Complexity: O(log n)
# Space Complexity: O(1)
class Solution:
    def myPow(self, x: float, n: int) -> float:
        result = 1.0
        current_product = x
        abs_n = abs(n)

        while abs_n > 0:
            if (abs_n % 2) == 1:
                result *= current_product
            current_product *= current_product
            abs_n //= 2

        return 1.0 / result if n < 0 else result
```

