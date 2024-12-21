# [Leetcode 202: Happy Number](https://leetcode.com/problems/happy-number/)

## Approaches

1. [Basic Approach: Using a Set to Track Visited Numbers](#basic-approach-using-a-set-to-track-visited-numbers)
2. [Optimal Approach: Floyd's Cycle Detection Algorithm](#optimal-approach-floyds-cycle-detection-algorithm)

---

### Basic Approach: Using a Set to Track Visited Numbers

#### Intuition

A "Happy Number" follows a sequence where we replace the number with the sum of the squares of its digits repeatedly. If this sequence ends in 1, the number is a happy number. Otherwise, it will eventually loop endlessly in a cycle that does not include 1. To determine if a number is happy, we can track numbers we have already computed using a set. If we reach a number that has already been seen, we are in a cycle and can conclude the number is not happy.

#### Steps

1. Use a set to record numbers that have already been encountered in the sequence.
2. Compute the sum of the squares of the digits of the current number.
3. Check if the result is 1 — if so, the number is happy.
4. If the result is already in the set, a cycle is detected, and the number is not happy.
5. Continue the process with the new number until you either reach 1 or encounter a cycle.

#### Code

```go
func isHappy(n int) bool {
    visit := make(map[int]bool)

    for n != 1 {
        if visit[n] {
            return false
        }
        visit[n] = true
        n = sumOfSquares(n)
    }
    return true
}

func sumOfSquares(n int) int {
    sum := 0
    for n > 0 {
        digit := n % 10
        sum += digit * digit
        n /= 10
    }
    return sum
}
```

#### Time Complexity
- \(O(\log n)\): For each number, we determine its sequence until reaching 1 or repeating a cycle. The cycle detection mechanism allows processing of each number at most once, and conversion to subsequent numbers takes logarithmic time in range of digits.

#### Space Complexity
- \(O(\log n)\): We store numbers seen so far in a map, which scales with sequence length.

---

### Optimal Approach: Floyd's Cycle Detection Algorithm

#### Intuition

By using Floyd’s Cycle Detection Algorithm (Tortoise and Hare), we can efficiently detect cycles within sequences by maintaining two pointers at different speeds. If there is a cycle, the fast pointer will eventually meet the slow pointer. If a cycle is detected without the fast pointer reaching 1, the number is not happy. Otherwise, if the fast pointer reaches 1, the number is confirmed as happy.

#### Steps

1. Initialize two pointers `slow` and `fast`, both starting at the input number.
2. Move `slow` pointer by one step (computing sum of squares once).
3. Move `fast` pointer by two steps (computing sum of squares twice).
4. If at any point `fast` equals 1, the number is happy.
5. If `slow` equals `fast`, a cycle is detected, confirming the number is not happy.
6. Continue until either condition is met.

#### Code

```go
func isHappy(n int) bool {
    slow, fast := n, n
    for {
        slow = sumOfSquares(slow)
        fast = sumOfSquares(sumOfSquares(fast))
        if fast == 1 {
            return true
        }
        if slow == fast {
            return false
        }
    }
}

func sumOfSquares(n int) int {
    sum := 0
    for n > 0 {
        digit := n % 10
        sum += digit * digit
        n /= 10
    }
    return sum
}
```

#### Time Complexity
- \(O(\log n)\): The algorithm still checks each number in the sequence proportional to its digit count, but effectively detects cycles sooner in the manner of Floyd's cycle algorithm.

#### Space Complexity
- \(O(1)\): No additional space is required aside from variables to store pointers and intermediate sums.

