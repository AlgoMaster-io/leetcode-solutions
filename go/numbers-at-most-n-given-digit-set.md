# [Leetcode 902: Numbers At Most N Given Digit Set](https://leetcode.com/problems/numbers-at-most-n-given-digit-set/)

## Solutions
- [Brute Force Approach](#brute-force-approach)
- [Mathematical Approach](#mathematical-approach)
- [Digits by Digits Approach](#digits-by-digits-approach)

### Brute Force Approach

**Intuition:**

The brute force method involves generating all numbers using the given digit set and counting only those numbers that are less than or equal to N. This is computationally expensive because it involves generating all permutations and combinations of digits up to N's length.

**Steps:**
1. Generate all possible numbers with lengths from 1 to the number of digits in N.
2. Check which of these numbers are less than or equal to N.
3. Count the valid numbers.

However, this approach is not feasible due to high complexity.

**Code:**

```go
func bruteForce(D []string, N int) int {
    // Convert the digit set D to integer set
    digits := make([]int, len(D))
    for i, d := range D {
        digits[i], _ = strconv.Atoi(d)
    }

    count := 0
    limit := N

    // Helper function to recursively build numbers
    var build func(cur int)
    build = func(cur int) {
        if cur > limit {
            return
        }
        count++
        
        for _, d := range digits {
            build(cur*10 + d)
        }
    }

    build(0)
    return count - 1 // Subtract 1 for initial call when cur is 0
}
```

**Complexity:**

- Time Complexity: Exponential due to generating all possible numbers.
- Space Complexity: O(1), apart from recursion stack.

### Mathematical Approach

**Intuition:**

Instead of generating all numbers, calculate how many numbers can be formed for each number of digits less than or equal to N's length.

**Steps:**
1. Count numbers that can be formed using digits of D up to length of N - 1.
2. For numbers of the same length as N, carefully construct them digit by digit and check for equality to avoid overcounting.

**Code:**

```go
func mathApproach(D []string, N int) int {
    strN := strconv.Itoa(N)
    nLen := len(strN)
    dLen := len(D)

    count := 0

    // Count numbers with digits less than the length of N
    for i := 1; i < nLen; i++ {
        count += int(math.Pow(float64(dLen), float64(i)))
    }

    // Helper to count valid numbers of the same length as N
    var countSameLength func(string, int) int
    countSameLength = func(prefix string, index int) int {
        if index == nLen {
            return 1
        }
        currentDigit := strN[index]
        total := 0
        
        for _, d := range D {
            if d[0] < currentDigit {
                total += int(math.Pow(float64(dLen), float64(nLen-index-1)))
            } else if d[0] == currentDigit {
                total += countSameLength(prefix+d, index+1)
            }
        }
        return total
    }

    count += countSameLength("", 0)
    return count
}
```

**Complexity:**

- Time Complexity: O(M^d) where M is the size of D and d is the number of digits in N.
- Space Complexity: O(d) for recursion.

### Digits by Digits Approach

**Intuition:**

This method involves breaking down the creation of numbers digit by digit. We carefully control the addition of each digit and ensure we don't exceed N prematurely.

**Steps:**
1. Process each digit position of N.
2. If a smaller digit can be placed, calculate all possible numbers with remaining digit slots.
3. Handle equal digits with care, ensuring we only proceed recursively valid additions.

**Code:**

```go
func digitsByDigits(D []string, N int) int {
    strN := strconv.Itoa(N)
    nLen := len(strN)
    dLen := len(D)

    count := 0

    // Count all numbers with length less than length of N
    for i := 1; i < nLen; i++ {
        count += int(math.Pow(float64(dLen), float64(i)))
    }

    // Count numbers with the same length as N
    for i := 0; i < nLen; i++ {
        match := false
        for _, d := range D {
            if d[0] < strN[i] {
                count += int(math.Pow(float64(dLen), float64(nLen-i-1)))
            } else if d[0] == strN[i] {
                match = true
                break
            }
        }
        if !match {
            return count
        }
    }
    return count + 1 // Include the number itself
}
```

**Complexity:**

- Time Complexity: O(M * d), M is the size of D, and d is the number of digits in N.
- Space Complexity: O(1)

---

Each approach adds a layer of optimization over the previous, with the 'Digits by Digits' method being the most efficient, allowing precise control over each digit's contribution.

