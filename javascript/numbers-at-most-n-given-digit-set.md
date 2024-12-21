# [Leetcode 902: Numbers At Most N Given Digit Set](https://leetcode.com/problems/numbers-at-most-n-given-digit-set/)

## Approaches
1. [Brute Force](#approach-1-brute-force)
2. [Mathematical Combinatorics](#approach-2-mathematical-combinatorics)

---

## Approach 1: Brute Force

### Intuition
The brute force approach involves generating numbers using all combinations of the given digits. For each generated number, we will check if it is less than or equal to `N`. This approach is straightforward but not efficient, especially when the size of the digit set and `N` are large.

### Detailed Steps
1. Generate all possible numbers using the given digit set.
2. For each generated number, convert it to an integer and check if it is less than or equal to `N`.
3. Count all numbers that satisfy the condition.

### Code
```javascript
function atMostNGivenDigitSet(D, N) {
  const numStr = N.toString(); 
  const numLength = numStr.length;
  const DLength = D.length;
  
  let count = 0;
  
  // Generate numbers for all lengths less than current number length.
  for (let i = 1; i < numLength; i++) {
    count += Math.pow(DLength, i);
  }
  
  const search = (index) => {
    if (index === numLength) return 1;

    const currentDigit = numStr[index];
    let currentCount = 0;

    for (let digit of D) {
      if (digit > currentDigit) break;
      if (digit === currentDigit) {
        currentCount += search(index + 1);
      } else {
        currentCount += Math.pow(DLength, numLength - index - 1);
      }
    }
    return currentCount;
  };

  count += search(0);
  
  return count;
}
```

### Complexity Analysis
- **Time Complexity:** O(N * 10^L) where N is the number of digits in `N` and L is the length of the digit set. Due to potentially generating lots of numbers.
- **Space Complexity:** O(1), if we consider call stack space, O(N).

---

## Approach 2: Mathematical Combinatorics

### Intuition
This approach leverages combinatorial counting methods. Instead of generating each number, we calculate how many numbers can be generated using the digit set for different lengths and positions.

### Detailed Steps
1. Count all possible numbers less than `N` with fewer digits.
2. Count numbers with the same number of digits as `N`, by checking digit by digit.
3. Use a recursive or iterative strategy to count the possible valid numbers.

### Code
```javascript
function atMostNGivenDigitSet(D, N) {
  const numStr = N.toString(); 
  const numLength = numStr.length;
  const DLength = D.length;
  
  let count = 0;
  
  // Count numbers with fewer digits than the number of digits in N
  for (let i = 1; i < numLength; i++) {
    count += Math.pow(DLength, i);
  }
  
  const search = (index) => {
    if (index === numLength) return 1;

    const currentDigit = numStr[index];
    let currentCount = 0;

    for (let digit of D) {
      if (digit > currentDigit) break;
      if (digit === currentDigit) {
        currentCount += search(index + 1);
      } else {
        currentCount += Math.pow(DLength, numLength - index - 1);
      }
    }
    return currentCount;
  };

  count += search(0);
  
  return count;
}
```

### Complexity Analysis
- **Time Complexity:** O(N log M), where N is the number of digits in `N` and M is the size of digit set D, due to counting process.
- **Space Complexity:** O(N), primarily due to recursion stack.

This solution is both optimal and efficient for the constraints provided in the problem.

