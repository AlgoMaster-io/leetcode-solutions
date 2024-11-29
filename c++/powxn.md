# 50. [Pow(x, n)](https://leetcode.com/problems/powx-n/)

## Approach 1: Iterative Exponentiation by Squaring

### Solution
java
```java
// Time Complexity: O(log n)
// Space Complexity: O(1)
public class Solution {
    public double myPow(double x, int n) {
        long N = n;
        if (N < 0) {
            x = 1 / x;
            N = -N;
        }
        
        double result = 1;
        double current_product = x;
        for (long i = N; i > 0; i /= 2) {
            if ((i % 2) == 1) {
                result = result * current_product;
            }
            current_product = current_product * current_product;
        }
        
        return result;
    }
}
```

c++
```cpp
// Time Complexity: O(log n)
// Space Complexity: O(1)
class Solution {
public:
    double myPow(double x, int n) {
        long N = n;
        if (N < 0) {
            x = 1 / x;
            N = -N;
        }
        
        double result = 1;
        double current_product = x;
        for (long i = N; i > 0; i /= 2) {
            if ((i % 2) == 1) {
                result *= current_product;
            }
            current_product *= current_product;
        }
        
        return result;
    }
};
```

## Approach 2: Recursive Exponentiation by Squaring

### Solution
java
```java
// Time Complexity: O(log n)
// Space Complexity: O(log n)
public class Solution {
    public double myPow(double x, int n) {
        long N = n;
        if (N < 0) {
            x = 1 / x;
            N = -N;
        }
        return fastPow(x, N);
    }
    
    private double fastPow(double x, long n) {
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

c++
```cpp
// Time Complexity: O(log n)
// Space Complexity: O(log n)
class Solution {
public:
    double myPow(double x, int n) {
        long N = n;
        if (N < 0) {
            x = 1 / x;
            N = -N;
        }
        return fastPow(x, N);
    }
    
private:
    double fastPow(double x, long n) {
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
};
```


