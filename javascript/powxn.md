# 50. [Pow(x, n)](https://leetcode.com/problems/powx-n/)

## Approach 1: Recursive Approach

### Solution
java
```java
// Time Complexity: O(log n)
// Space Complexity: O(log n)
class Solution {
    public double myPow(double x, int n) {
        if (n == 0) {
            return 1;
        }

        // Recursive call
        double half = myPow(x, n / 2);

        // Combine results for even and odd n
        if (n % 2 == 0) {
            return half * half;
        } else {
            return n > 0 ? half * half * x : half * half / x;
        }
    }
}
```

### Solution
javascript
```javascript
// Time Complexity: O(log n)
// Space Complexity: O(log n)
var myPow = function(x, n) {
    if (n === 0) {
        return 1;
    }

    // Recursive call
    const half = myPow(x, Math.floor(n / 2));

    // Combine results for even and odd n
    if (n % 2 === 0) {
        return half * half;
    } else {
        return n > 0 ? half * half * x : half * half / x;
    }
};
```

## Approach 2: Iterative Approach

### Solution
java
```java
// Time Complexity: O(log n)
// Space Complexity: O(1)
class Solution {
    public double myPow(double x, int n) {
        long N = n;
        if (N < 0) {
            x = 1 / x;
            N = -N;
        }

        double result = 1;
        double currentProduct = x;

        for (long i = N; i > 0; i /= 2) {
            if ((i % 2) == 1) {
                result *= currentProduct;
            }
            currentProduct *= currentProduct;
        }

        return result;
    }
}
```

### Solution
javascript
```javascript
// Time Complexity: O(log n)
// Space Complexity: O(1)
var myPow = function(x, n) {
    let N = n;
    if (N < 0) {
        x = 1 / x;
        N = -N;
    }

    let result = 1;
    let currentProduct = x;

    for (let i = N; i > 0; i = Math.floor(i / 2)) {
        if (i % 2 === 1) {
            result *= currentProduct;
        }
        currentProduct *= currentProduct;
    }

    return result;
};
```

