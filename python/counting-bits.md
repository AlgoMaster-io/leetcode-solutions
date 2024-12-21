# [338. Counting Bits](https://leetcode.com/problems/counting-bits/)

## Approaches

1. [Naive Approach](#naive-approach)
2. [Dynamic Programming - Transition based on Last Computation](#dynamic-programming---transition-based-on-last-computation)
3. [Optimal Dynamic Programming - Most Significant Bit](#optimal-dynamic-programming---most-significant-bit)

---

### Naive Approach

To find the number of 1s in the binary representation of each number from 0 to n.

#### Intuition

The simplest approach is to convert each number to its binary form and count the 1s. This can be done using Python's inbuilt functions where we convert a number to a binary string and count '1's.

#### Code

```python
def countBits(n: int) -> List[int]:
    result = []
    for i in range(n + 1):
        # Convert the number to binary using bin() and count '1's
        count = bin(i).count('1')
        result.append(count)
    return result
```

#### Complexity

- **Time Complexity**: \(O(n \cdot \log k)\), where \(k\) is the maximum number in the range, to convert each number to binary form.
- **Space Complexity**: \(O(1)\) additional space, output array is not considered.

---

### Dynamic Programming - Transition based on Last Computation

#### Intuition

The number of 1s for any number `i` can be calculated using results from smaller numbers. Specifically, if `i` is even, its 1s bits are the same as `i//2`. If `i` is odd, it's 1 more than `i-1`.

- When `i` is even: `bits[i] = bits[i >> 1]`
- When `i` is odd: `bits[i] = bits[i - 1] + 1`

This is because shifting an even number to the right by one bit corresponds to dividing by 2, which doesn't change the number of 1s.

#### Code

```python
def countBits(n: int) -> List[int]:
    dp = [0] * (n + 1)
    for i in range(1, n + 1):
        dp[i] = dp[i // 2] + (i % 2)
    return dp
```

#### Complexity

- **Time Complexity**: \(O(n)\), as we compute results for each number up to n.
- **Space Complexity**: \(O(n)\), due to the dp array storage.

---

### Optimal Dynamic Programming - Most Significant Bit

#### Intuition

This optimization is based on the observation that if you know the most significant bit of a number, the rest is just like a smaller number that you have already solved for.

When a number's highest bit, say at `m`, is 1, you can represent it as a combination of 2^m and some smaller number `x`. The number of 1s for this combination is then `1 + countBits(x)`.

This provides an even more efficient approach:

- Identify the most significant bit that is a power of 2.
- For a number `i`, its bits count is `1 + bits[i - power]` where `power` is the largest power of 2 less than `i`.

#### Code

```python
def countBits(n: int) -> List[int]:
    dp = [0] * (n + 1)
    offset = 1  # to track the latest power of two
    for i in range(1, n + 1):
        # update offset when we get to the next power of two
        if offset * 2 == i:
            offset = i
        dp[i] = 1 + dp[i - offset]
    return dp
```

#### Complexity

- **Time Complexity**: \(O(n)\), because we iterate over each number up to n.
- **Space Complexity**: \(O(n)\), for the storage of the dp array.

---

Each of these methods effectively calculates the number of 1 bits for each integer in the range 0 to n, trading off complexity of implementation versus runtime efficiency. Implementing the optimal DP approach offers a significantly more effective solution with \(O(n)\) time and space, leveraging insights from previous computations.

