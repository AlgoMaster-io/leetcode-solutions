# [Leetcode 172: Factorial Trailing Zeroes](https://leetcode.com/problems/factorial-trailing-zeroes/)

## Approaches
- [Iterative Division by 5](#approach-1-iterative-division-by-5)

---

## Approach 1: Iterative Division by 5

### Intuition
The trailing zeroes in a factorial are produced by pairs of factors of 10. Each 10 is composed of 2 * 5. In any factorial, there are always more 2s than 5s, thus the number of trailing zeroes is determined by the number of 5s.

For a number `n`, count how many times `5` is a factor for all numbers from `1` to `n`. Note that 25 contributes an extra zero because it can be divided by 5 twice (it's 5 * 5), 125 contributes two additional zeroes (5 * 5 * 5), etc.

The key realization is that you don't actually need to compute the factorial itself, just the count of 5s.

### Code
```cpp
class Solution {
public:
    int trailingZeroes(int n) {
        int count = 0;
        
        // Keep dividing n by 5 and add the quotients
        while (n >= 5) {
            n /= 5;
            count += n;
        }
        
        return count;
    }
};
```

### Explanation
1. **Initialize**: Start with a zero counter to accumulate the count of 5s.
2. **Iterative Division**: Divide `n` by 5 to count the number of multiples of 5 up to `n`.
3. **Accumulate**: Add up all the quotients. This operation effectively counts all the trailing zeroes by counting factors of 5.
4. **Return**: The accumulated count.

### Complexity Analysis
- **Time Complexity**: O(log_5(n)), where each division by 5 reduces `n`.
- **Space Complexity**: O(1), as we're only using a few variables.

By understanding that trailing zeroes are simply a count of factor 5s, we can efficiently solve this problem without heavy computation or large integer management.

