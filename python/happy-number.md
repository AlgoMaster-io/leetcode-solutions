# [LeetCode 202: Happy Number](https://leetcode.com/problems/happy-number/)

## Approaches:
- [Approach 1: Using HashSet](#approach-1-using-hashset)
- [Approach 2: Floyd's Cycle-Finding Algorithm (Optimal)](#approach-2-floyds-cycle-finding-algorithm-optimal)

## Approach 1: Using HashSet

### Intuition:
A happy number is a number which eventually reaches `1` when replaced by the sum of the square of each digit repeatedly. If this process results in an endless cycle that does not include `1`, the number is not happy. We can detect cycles using a hashset, where we store all the intermediate numbers we've seen so far. If we encounter a number we've seen before, it means we're in a cycle and the number is not happy.

### Implementation:

```python
def isHappy(n: int) -> bool:
    def get_next(number):
        total_sum = 0
        while number > 0:
            number, digit = divmod(number, 10)
            total_sum += digit ** 2
        return total_sum

    seen = set()
    while n != 1 and n not in seen:
        seen.add(n)  # Add intermediate number to the set
        n = get_next(n)  # Update n to the sum of squares of its digits

    return n == 1

# Example usage:
# print(isHappy(19))  # Output: True
```

### Time Complexity:
- **O(log n)**: Since each step roughly reduces n by half, the number of steps is logarithmic in the number of digits.
### Space Complexity:
- **O(log n)**: In the worst case, we need to store sets of all the numbers encountered, which would be proportional to the number of digits.

## Approach 2: Floyd's Cycle-Finding Algorithm (Optimal)

### Intuition:
The number transformation can be seen as a sequence of states. If there exists a cycle, we will never reach `1`. We can use the tortoise and hare method (Floyd's Cycle-Finding) to detect cycles efficiently without using extra space.

### Implementation:

```python
def isHappy(n: int) -> bool:
    def get_next(number):
        total_sum = 0
        while number > 0:
            number, digit = divmod(number, 10)
            total_sum += digit ** 2
        return total_sum

    slow = n
    fast = get_next(n)

    while fast != 1 and slow != fast:
        slow = get_next(slow)  # Move slow by one step
        fast = get_next(get_next(fast))  # Move fast by two steps

    return fast == 1

# Example usage:
# print(isHappy(19))  # Output: True
```

### Time Complexity:
- **O(log n)**: The function `get_next` is logarithmic due to the number of digits.
### Space Complexity:
- **O(1)**: Only a constant amount of extra space is used.

By using Floyd’s algorithm, we save on space and efficiently detect cycles, making this the optimal solution for this problem.

