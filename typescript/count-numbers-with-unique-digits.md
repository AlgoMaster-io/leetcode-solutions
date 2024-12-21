# [357. Count Numbers with Unique Digits](https://leetcode.com/problems/count-numbers-with-unique-digits/)

## Approaches

- [Brute Force Approach](#brute-force-approach)
- [Dynamic Programming Approach](#dynamic-programming-approach)

## Brute Force Approach

### Intuition

The brute force approach involves generating all numbers up to 10^n and counting how many of them have all unique digits. This is a straightforward solution, but it is not efficient for larger values of `n` due to the number of numbers that need to be checked.

### Steps

1. **Generate Numbers**: For each number from 0 to \(10^n - 1\), check if all digits are unique.
2. **Check Uniqueness**: Convert the number to a string and use a set to track digits. If the length of the set equals the length of the string, all digits are unique.
3. **Count Unique**: Count numbers that pass the uniqueness test.

### Code

```typescript
function countNumbersWithUniqueDigits(n: number): number {
    if (n === 0) return 1;

    let uniqueCount = 0;
    
    const isUniqueDigitNumber = (num: number): boolean => {
        const strNum = num.toString();
        const digitSet = new Set<string>();
        for (const digit of strNum) {
            if (digitSet.has(digit)) {
                return false;
            }
            digitSet.add(digit);
        }
        return true;
    };

    for (let i = 0; i < Math.pow(10, n); i++) {
        if (isUniqueDigitNumber(i)) {
            uniqueCount++;
        }
    }

    return uniqueCount;
}
```

### Time and Space Complexity

- **Time Complexity**: \(O(10^n \cdot n)\) - Checking all numbers up to \(10^n\) and using a set for each number with approximately `n` digits.
- **Space Complexity**: \(O(n)\) - For the set used in uniqueness checking.

## Dynamic Programming Approach

### Intuition

Instead of exact counting, use combinatorics to calculate possibilities of forming unique digit numbers. For instance, for \(n = 1\), numbers range from 0 to 9 (10 numbers); for \(n = 2\), the first digit has 9 options (1-9) and the second has 9 options (0-9 except the first digit). This can be calculated using permutation logic.

### Steps

1. **Start with Basic Cases**: Base case for `n = 0` is 1 (only the number 0).
2. **Calculate Unique Digits**: Use permutations to calculate valid choices of digits.
3. **Iterate through `n`**: Accumulate the count of unique numbers for each step up to `n`.

### Code

```typescript
function countNumbersWithUniqueDigits(n: number): number {
    if (n === 0) return 1;

    let ans = 10; // for n = 1, all numbers 0-9 are unique
    let uniqueDigits = 9;
    let availableDigits = 9;

    // Calculating the permutation for numbers until length `n`
    while (n > 1 && availableDigits > 0) {
        uniqueDigits *= availableDigits;
        ans += uniqueDigits;
        availableDigits--;
        n--;
    }

    return ans;
}
```

### Time and Space Complexity

- **Time Complexity**: \(O(n)\) - Linear operation over digit possibilities.
- **Space Complexity**: \(O(1)\) - Constant space usage. 

These solutions demonstrate how leveraging combinatorial principles can significantly reduce computation for problems with large input sizes, making the dynamic programming approach much more efficient for larger inputs.

