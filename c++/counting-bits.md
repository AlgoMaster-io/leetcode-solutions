# [Counting Bits](https://leetcode.com/problems/counting-bits/)

## Approaches
1. [Brute Force](#brute-force)
2. [Dynamic Programming - Using Half and LSB](#dynamic-programming-using-half-and-lsb)
3. [Dynamic Programming - Using last set bit](#dynamic-programming-using-last-set-bit)

---

### Brute Force

The most straightforward approach to this problem is to count the number of `1`s in the binary representation of each number from `0` to `n`.

#### Intuition

For every number, convert the number to binary, count the `1`s and store in the result. Though simple, this method is not efficient especially for higher values of `n`.

```cpp
#include <vector>

std::vector<int> countBits(int num) {
    std::vector<int> result(num + 1, 0);
    for (int i = 0; i <= num; ++i) {
        int count = 0;
        int n = i;
        // Count 1s in binary representation
        while (n) {
            count += n & 1; // Check if the last bit is 1
            n >>= 1; // Shift right by 1 bit
        }
        result[i] = count;
    }
    return result;
}
```

#### Complexity
- **Time Complexity**: O(n * log(n)), where n is the given number. For each number, we check all its binary digits.
- **Space Complexity**: O(n), where n is the storage for the result.

---

### Dynamic Programming Using Half and LSB

We can use previous results to compute the new values efficiently using the observation that the count of `1`s for a number `i` can be derived from `i / 2` by adding `i % 2`.

#### Intuition

1. For even `i` (`i = 2 * k`), the binary representation has the same number of `1`s as `k`.
2. For odd `i` (`i = 2 * k + 1`), the count is `1` more than `k` as this additional bit is `1`.

This gives the relation: `countBits[i] = countBits[i / 2] + (i % 2)`.

```cpp
#include <vector>

std::vector<int> countBits(int num) {
    std::vector<int> result(num + 1, 0);
    for (int i = 1; i <= num; ++i) {
        result[i] = result[i >> 1] + (i & 1);  // i >> 1 is i / 2, i & 1 checks if odd
    }
    return result;
}
```

#### Complexity
- **Time Complexity**: O(n), since we are computing the result directly using previously computed results.
- **Space Complexity**: O(n), for storing the results.

---

### Dynamic Programming Using Last Set Bit

Another efficient way is by using the relation which is based on the most significant set bit that was turned off for the last set bit turned on.

#### Intuition

From the observation, the number of `1`s in `i` is `1` more than `i & (i - 1)` (turning off the last significant bit).

This gives us: `countBits[i] = countBits[i & (i - 1)] + 1`.

```cpp
#include <vector>

std::vector<int> countBits(int num) {
    std::vector<int> result(num + 1, 0);
    for (int i = 1; i <= num; ++i) {
        result[i] = result[i & (i - 1)] + 1;  // i & (i - 1) removes the last significant 1
    }
    return result;
}
```

#### Complexity
- **Time Complexity**: O(n), efficient computation using relationship.
- **Space Complexity**: O(n), for the result array.

These dynamic programming approaches provide a much more efficient way to solve the problem compared to the brute force. The choice between them often comes down to personal or situational preference as they have the same time and space complexity.

