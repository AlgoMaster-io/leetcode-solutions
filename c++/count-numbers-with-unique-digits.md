# [Leetcode 357: Count Numbers with Unique Digits](https://leetcode.com/problems/count-numbers-with-unique-digits/)

## Approaches
1. [Brute Force Enumeration](#approach-1-brute-force-enumeration)
2. [Mathematical Approach Using Permutations](#approach-2-mathematical-approach-using-permutations)

---

## Approach 1: Brute Force Enumeration

### Intuition
The simplest approach is to iterate over all the numbers from `0` to `10^n - 1` and count the numbers that have unique digits. Check each number to ensure all digits are distinct.

### Code
```cpp
#include <vector>
#include <cmath>
#include <iostream>

bool hasUniqueDigits(int num) {
    std::vector<int> digit_count(10, 0);
    while (num > 0) {
        int digit = num % 10;
        if (digit_count[digit] > 0) {
            return false;
        }
        digit_count[digit]++;
        num /= 10;
    }
    return true;
}

int countNumbersWithUniqueDigits(int n) {
    int upperLimit = pow(10, n);
    int uniqueCount = 0;
    
    for (int i = 0; i < upperLimit; ++i) {
        if (hasUniqueDigits(i)) {
            uniqueCount++;
        }
    }
    
    return uniqueCount;
}

int main() {
    int n = 2;
    std::cout << countNumbersWithUniqueDigits(n) << std::endl;
    return 0;
}
```

### Time Complexity
- The time complexity is O(10^n * d) where d is the number of digits which is a constant factor in the range 1-10.
  
### Space Complexity
- The space complexity is O(1) since we're using a constant space for the digit count.

---

## Approach 2: Mathematical Approach Using Permutations

### Intuition
Another efficient way is using a mathematical approach to calculate the number of unique-digit numbers using permutations. For a number with unique digits, the choice for each digit is independent of previous choices. 

- For 1-digit numbers: There are 10 options (0-9).
- For 2-digit numbers: The first digit has 9 options (1-9, since it can't be 0), and the second digit has 9 options (0-9, except the first digit).
- For 3-digit numbers: The first digit has 9 options (1-9), the second digit has 9 options, and the third digit has 8 options.

The pattern continues up to n digits. We accumulate the sum of all unique numbers for 1 to n digits.

### Code
```cpp
#include <iostream>
#include <vector>

int countNumbersWithUniqueDigits(int n) {
    if (n == 0) return 1;
    if (n == 1) return 10;
    
    int result = 10; // count for n = 1
    int unique_digit_count = 9;
    int available_digits = 9;
    
    for (int i = 2; i <= n && available_digits > 0; ++i) {
        unique_digit_count *= available_digits;
        result += unique_digit_count;
        available_digits--;
    }
    
    return result;
}

int main() {
    int n = 2;
    std::cout << countNumbersWithUniqueDigits(n) << std::endl;
    return 0;
}
```

### Time Complexity
- The time complexity is O(n) since we iterate up to n which is a small constant (maximum 10).

### Space Complexity
- The space complexity is O(1) since only a fixed number of variables are used regardless of the input size.

---

