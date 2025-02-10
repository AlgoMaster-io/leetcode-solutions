# [Leetcode 338: Counting Bits](https://leetcode.com/problems/counting-bits/)

## Approaches
- [Leetcode 338: Counting Bits](#leetcode-338-counting-bits)
  - [Approaches](#approaches)
    - [Approach 1: Naive Approach](#approach-1-naive-approach)
      - [Intuition](#intuition)
      - [Code](#code)
      - [Time Complexity](#time-complexity)
      - [Space Complexity](#space-complexity)
    - [Approach 2: DP with Last Significant Bit](#approach-2-dp-with-last-significant-bit)
      - [Intuition](#intuition-1)
      - [Code](#code-1)
      - [Time Complexity](#time-complexity-1)
      - [Space Complexity](#space-complexity-1)
    - [Approach 3: DP with Highest Power of Two](#approach-3-dp-with-highest-power-of-two)
      - [Intuition](#intuition-2)
      - [Code](#code-2)
      - [Time Complexity](#time-complexity-2)
      - [Space Complexity](#space-complexity-2)

---

### Approach 1: Naive Approach

#### Intuition
The naive approach involves iterating over each number from 0 to n and calculating the number of 1's in its binary representation. This can be achieved by repeatedly dividing the number by 2 and counting the remainder when divided by 2.

#### Code
```java
public int[] countBits(int n) {
    int[] res = new int[n + 1];
    
    for (int i = 0; i <= n; i++) {
        int count = 0;
        int num = i;
        // Count the number of 1's in the binary representation of 'i'
        while (num > 0) {
            count += num % 2; // Add 1 to count if the least-significant bit is set
            num /= 2; // Right shift the number
        }
        res[i] = count;
    }
    return res;
}
```

#### Time Complexity
- O(n * log(n)) because for each number up to n, we are performing shifts proportional to log(n).

#### Space Complexity
- O(n) for the output array.

---

### Approach 2: DP with Last Significant Bit

#### Intuition
A more efficient approach is to use dynamic programming. For each number `i`, the number of 1's can be derived from `i >> 1` (i.e., `i / 2`), plus `i % 2`. This is because shifting a number right by one position reduces it to half of its value while still providing a mechanism to determine if the least significant bit of `i` was a 1.

#### Code
```java
public int[] countBits(int n) {
    int[] res = new int[n + 1];
    res[0] = 0;
    
    for (int i = 1; i <= n; i++) {
        // Use results from smaller numbers and add 1 if the current number is odd
        res[i] = res[i >> 1] + (i & 1);
    }
    return res;
}
```

#### Time Complexity
- O(n) because we compute the number of bits for each number up to n exactly once.

#### Space Complexity
- O(n) for the output array.

---

### Approach 3: DP with Highest Power of Two

#### Intuition
This method involves finding the relationship between subsequent powers of two. For any number `x`, if `x` lies between 2^m and 2^(m+1), it can be expressed as `2^m + k`, where `0 <= k < 2^m`. Thus, the number of 1's in `x` is 1 plus the number of 1's in `k`, allowing us to reuse previously computed results efficiently.

#### Code
```java
public int[] countBits(int n) {
    int[] res = new int[n + 1];
    int pow = 1; // Current power of two
    int x = 1; // Index at the current power level

    for (int i = 1; i <= n; i++) {
        if (i == pow) {
            pow *= 2; // Move to the next power of two
            x = i;
        }
        res[i] = res[i - x] + 1; // Use the result from a smaller index plus one
    }
    return res;
}
```

#### Time Complexity
- O(n) because we are iterating over each element only once.

#### Space Complexity
- O(n) for the output array. 

This approach efficiently leverages previously calculated values, resulting in optimized computation compared to basic methods while maintaining the simplicity of a dynamic programming solution.

