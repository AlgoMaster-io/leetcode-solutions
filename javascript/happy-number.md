# [Leetcode 202: Happy Number](https://leetcode.com/problems/happy-number/)

## Approaches:
1. [Basic Approach: Cycle Detection with HashSet](#basic-approach-cycle-detection-with-hashset)
2. [Optimal Approach: Floyd's Cycle Detection Algorithm](#optimal-approach-floyds-cycle-detection-algorithm)

---

## Basic Approach: Cycle Detection with HashSet

### Intuition:
A "happy number" is defined as a number which eventually reaches 1 when replaced by the sum of the square of its digits repeatedly. Numbers that aren't happy will eventually form a cycle that does not include 1. By using a HashSet, we can detect when a cycle occurs. If we reach a number we've seen before, we know we're in a cycle and can return false. If we reach 1, we know the number is happy.

### Code:
```javascript
function isHappy(n) {
    // Helper function to calculate the sum of the square of digits of a number
    function sumOfSquares(num) {
        let sum = 0;
        while (num > 0) {
            let digit = num % 10; // extract the last digit
            sum += digit * digit; // square the digit and add to sum
            num = Math.floor(num / 10); // remove the last digit
        }
        return sum;
    }
    
    const seen = new Set(); // Set to keep track of numbers we've encountered
    
    while (n !== 1 && !seen.has(n)) {
        seen.add(n); // mark the current number as seen
        n = sumOfSquares(n); // replace n with the sum of the square of its digits
    }
    
    // If we've exited the loop and n is 1, it's a happy number; otherwise, it's not
    return n === 1;
}
```

### Time Complexity:
- **O(k)**: Where k is the number of iterations before a loop is detected or we reach 1. Since the sum of squares operation reduces the number, this is generally quite efficient.
  
### Space Complexity:
- **O(k)**: Due to the space required to store the numbers in the set.

---

## Optimal Approach: Floyd's Cycle Detection Algorithm

### Intuition:
An optimal way to detect cycles is using Floyd's Cycle Detection (Tortoise and Hare) algorithm, more commonly used in detecting cycles in linked lists. This algorithm uses two pointers, moving at different speeds, and can determine if a cycle exists without needing to store previous values.

### Code:
```javascript
function isHappy(n) {
    // Helper function to calculate the sum of the square of digits of a number
    function sumOfSquares(num) {
        let sum = 0;
        while (num > 0) {
            let digit = num % 10;
            sum += digit * digit;
            num = Math.floor(num / 10);
        }
        return sum;
    }
    
    let slow = n;
    let fast = sumOfSquares(n);
    
    // Continue until fast equals 1 (happy number) or slow equals fast (cycle)
    while (fast !== 1 && slow !== fast) {
        slow = sumOfSquares(slow); // move slow pointer by one step
        fast = sumOfSquares(sumOfSquares(fast)); // move fast pointer by two steps
    }

    // If fast equals 1, it's a happy number
    return fast === 1;
}
```

### Time Complexity:
- **O(k)**: Similar to the HashSet approach, where k is the number of iterations to reach a loop or 1.

### Space Complexity:
- **O(1)**: Only constant extra space is used, as no data structures are needed to store intermediate numbers.

---

These approaches provide ways to determine if a number is happy, with the second method being more optimized for space, avoiding the use of additional memory.

