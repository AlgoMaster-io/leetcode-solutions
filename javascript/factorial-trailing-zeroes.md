# [LeetCode 172: Factorial Trailing Zeroes](https://leetcode.com/problems/factorial-trailing-zeroes/)

## Approaches:

- [Brute Force Approach](#brute-force-approach)
- [Counting Factors of 5 Approach](#counting-factors-of-5-approach)

### Brute Force Approach

#### Intuition:
The brute force approach would involve calculating the factorial of a number and then counting the number of trailing zeroes in it. The trailing zeroes are introduced by the factor 10, which is the product of 2 and 5. Since there are generally more factors of 2 than 5 in factorial numbers, the number of trailing zeroes is equivalent to the number of times 5 is a factor in the numbers from 1 to `n`.

#### Steps:
1. Calculate the factorial of the number `n`.
2. Convert the result to a string to count trailing zeroes.
3. Count the zeroes starting from the end of the string.

#### Code:
```javascript
function factorialTrailingZeroesBruteForce(n) {
    // Calculate factorial of n
    let factorial = 1;
    for (let i = 2; i <= n; i++) {
        factorial *= i;
    }

    // Convert factorial to string and check trailing zeroes
    let factorialStr = factorial.toString();
    let trailingZeroes = 0;

    // Count trailing zeroes from end of the string
    for (let i = factorialStr.length - 1; i >= 0; i--) {
        if (factorialStr[i] === '0') {
            trailingZeroes++;
        } else {
            break;
        }
    }
    return trailingZeroes;
}

// Example usage:
console.log(factorialTrailingZeroesBruteForce(5)); // Output: 1
```

#### Time and Space Complexity:
- **Time Complexity**: O(n), due to the calculation of the factorial.
- **Space Complexity**: O(1), as we only use a handful of variables.

#### Limitation:
- This solution is inefficient and impractical for larger values of `n`. Factorials grow extremely fast, resulting in very large numbers.

### Counting Factors of 5 Approach

#### Intuition:
We can directly calculate the number of trailing zeroes by counting the factors of 5 in the numbers from 1 to `n`. Since each factor of 5 paired with a factor of 2 forms a trailing zero, the number of trailing zeroes is determined by the number of factors of 5.

#### Steps:
1. Initialize a count of zeroes to 0.
2. Start with the multiple of 5 (i.e., `n/5`).
3. Continuously divide `n` by powers of 5 to count how many multiples of 5, 25, 125, etc., are there up to `n`.

#### Code:
```javascript
function factorialTrailingZeroesEfficient(n) {
    let zeroes = 0;
    while (n >= 5) {
        // Divide n by 5 and add to zeroes count
        n = Math.floor(n / 5);
        zeroes += n;
    }
    return zeroes;
}

// Example usage:
console.log(factorialTrailingZeroesEfficient(100)); // Output: 24
```

#### Time and Space Complexity:
- **Time Complexity**: O(log₅(n)), because we're dividing `n` by 5 iteratively.
- **Space Complexity**: O(1), as we only use a fixed number of auxiliary variables.

#### Benefit:
- This solution is very efficient and can handle large values of `n` up to the problem constraints effortlessly.

