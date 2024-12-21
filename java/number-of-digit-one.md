## [Leetcode 233: Number of Digit One](https://leetcode.com/problems/number-of-digit-one/)

In this problem, we are tasked to determine how many times the digit `1` appears in the numbers from `0` to `n`.

### Approach 1: Brute Force

This basic solution involves iterating through each number from 1 to `n` and counting the number of times the digit `1` appears. Though simple, this approach lacks efficiency.

#### Intuition

For each number, convert it to a string and count the occurrences of '1'. Sum the counts across all numbers.

#### Implementation

```java
public int countDigitOne(int n) {
    int count = 0;
    for (int i = 1; i <= n; i++) {
        count += countOnesInNumber(i);
    }
    return count;
}

private int countOnesInNumber(int num) {
    int count = 0;
    while (num > 0) {
        if (num % 10 == 1) {
            count++;
        }
        num /= 10;
    }
    return count;
}
```

#### Complexity Analysis

- **Time Complexity**: \(O(n \cdot \log_{10}(n))\) — We check each number and its digits.
- **Space Complexity**: \(O(1)\) — Constant space is used.

### Approach 2: Mathematical Solution

While the brute-force solution checks each number individually, an optimal approach models a mathematical pattern based on the position of each digit.

#### Intuition

Consider how many 1s appear at each digit through the entire sequence from `0` to `n`. By calculating the contribution of 1s for units, tens, hundreds places independently, a more efficient solution is derived.

For each digit in the number, split the number into three parts: higher, current, and lower part. Compute possible occurrences of 1 considering each position as the current digit.

#### Implementation

```java
public int countDigitOne(int n) {
    int count = 0;
    for (long digitFactor = 1; digitFactor <= n; digitFactor *= 10) {
        long higherPart = n / (digitFactor * 10);
        long currentDigit = (n / digitFactor) % 10;
        long lowerPart = n % digitFactor;
        
        // Add the ones contributed by the higher part
        count += higherPart * digitFactor;
        
        // If the current digit is 1, add all the numbers formed by lower digits + 1
        if (currentDigit == 1) {
            count += lowerPart + 1;
        } 
        // If the current digit is greater than 1, it forms complete sets of 0 to 9, hence add the whole digitFactor
        else if (currentDigit > 1) {
            count += digitFactor;
        }
    }
    return count;
}
```

#### Complexity Analysis

- **Time Complexity**: \(O(\log_{10}(n))\) — The number of iterations is based on the number of digits in `n`.
- **Space Complexity**: \(O(1)\) — Only a few variables are used.

This mathematical analysis is both time efficient and easy to understand once the concept of counting by digit place is grasped. Each digit is treated independently, and computations focus on groups of numbers defined by those digits rather than each number itself.

