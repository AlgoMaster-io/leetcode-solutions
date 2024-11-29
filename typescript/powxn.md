# 50. [Pow(x, n)](https://leetcode.com/problems/powx-n/)

## Approach 1: Recursive Approach

### Solution
java
```java
// Time Complexity: O(log n)
// Space Complexity: O(log n)
public class Solution {
    public double myPow(double x, int n) {
        if (n == 0) {
            return 1.0;
        }

        double half = myPow(x, n / 2);

        if (n % 2 == 0) {
            return half * half;
        } else {
            if (n > 0) {
                return half * half * x;
            } else {
                return half * half / x;
            }
        }
    }
}
```

typescript
```typescript
// Time Complexity: O(log n)
// Space Complexity: O(log n)
class Solution {
    myPow(x: number, n: number): number {
        if (n === 0) {
            return 1.0;
        }

        const half = this.myPow(x, Math.floor(n / 2));

        if (n % 2 === 0) {
            return half * half;
        } else {
            if (n > 0) {
                return half * half * x;
            } else {
                return half * half / x;
            }
        }
    }
}
```

## Approach 2: Iterative Approach

### Solution
java
```java
// Time Complexity: O(log n)
// Space Complexity: O(1)
public class Solution {
    public double myPow(double x, int n) {
        long exp = n;
        if (exp < 0) {
            x = 1 / x;
            exp = -exp;
        }

        double result = 1.0;
        while (exp > 0) {
            if ((exp % 2) != 0) {
                result *= x;
            }
            x *= x;
            exp /= 2;
        }

        return result;
    }
}
```

typescript
```typescript
// Time Complexity: O(log n)
// Space Complexity: O(1)
class Solution {
    myPow(x: number, n: number): number {
        let exp = n;
        if (exp < 0) {
            x = 1 / x;
            exp = -exp;
        }

        let result = 1.0;
        while (exp > 0) {
            if ((exp % 2) !== 0) {
                result *= x;
            }
            x *= x;
            exp = Math.floor(exp / 2);
        }

        return result;
    }
}
```

