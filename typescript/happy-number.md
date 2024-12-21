# [Leetcode 202: Happy Number](https://leetcode.com/problems/happy-number/)

## Approaches:
1. [Brute Force with HashSet](#brute-force-with-hashset)
2. [Floyd's Cycle Detection Algorithm (Optimal)](#floyds-cycle-detection-algorithm-optimal)

---

### Brute Force with HashSet

**Intuition**:  
The problem is to determine if a given number is a "happy number". A happy number is a number which eventually reaches 1 when replaced by the sum of the square of its digits repeatedly. If a number isn’t happy, this process will result in an infinite loop which there's the need to detect.

The simplest way to solve this is by keeping track of each number we get during the process in a HashSet. If we encounter a number that we have already seen, we conclude it's a cycle (and not a happy number) which won't reach 1.

**Steps**:
1. Convert the number into its digits, square each digit, and sum them up to form a new number.
2. Use a HashSet to track all numbers we've processed.
3. If the new number is already in the HashSet, we are in a cycle.
4. If the number becomes 1, it’s a happy number.

```typescript
function isHappy(n: number): boolean {
    // Initialize a set to store numbers to detect cycles
    const seen = new Set<number>();
    
    // Function to calculate the sum of squares of digits of `num`
    function getNext(num: number): number {
        let totalSum = 0;
        while (num > 0) {
            const digit = num % 10;
            totalSum += digit * digit;
            num = Math.floor(num / 10);
        }
        return totalSum;
    }
    
    // Repeat until we either find a cycle or reach 1
    while (n !== 1 && !seen.has(n)) {
        seen.add(n); // Track the seen number
        n = getNext(n); // Get the next number in the sequence
    }
    
    // If the resulting number is 1, it's a happy number
    return n === 1;
}
```

**Time Complexity**: O(log n). In the worst case, each time we calculate getNext results in a number with fewer digits.  
**Space Complexity**: O(log n), due to the storage of seen values in a HashSet.

---

### Floyd's Cycle Detection Algorithm (Optimal)

**Intuition**:  
An optimized approach doesn't need to store the history of all numbers. Instead, we can use the "Floyd's Cycle detection algorithm", also known as the Tortoise and Hare algorithm. This algorithm is used to detect cycles in a sequence efficiently.

By applying this algorithm:
- Use two pointers, `slow` and `fast`.
- `Slow` advances by one step (calling getNext once).
- `Fast` advances by two steps (calling getNext twice).
- If there's a cycle, both pointers will eventually meet.
- If a pointer reaches 1, it's a happy number.

**Steps**:
1. Initialize two pointers, both starting at the initial number.
2. Use the `getNext()` function for iterating over numbers.
3. Advance `slow` pointer by one step and `fast` by two steps.
4. If the fast pointer reaches 1, it is a happy number. If both pointers meet (other than at 1), there is a cycle.

```typescript
function isHappy(n: number): boolean {
    // Function to calculate the sum of squares of digits of `num`
    function getNext(num: number): number {
        let totalSum = 0;
        while (num > 0) {
            const digit = num % 10;
            totalSum += digit * digit;
            num = Math.floor(num / 10);
        }
        return totalSum;
    }
    
    // Initializing the two pointers
    let slow = n;
    let fast = getNext(n);
    
    // Continue until fast meets slow or fast reaches 1
    while (fast !== 1 && slow !== fast) {
        slow = getNext(slow); // Advance slow by 1 step
        fast = getNext(getNext(fast)); // Advance fast by 2 steps
    }
    
    // If fast reached 1, it is a happy number
    return fast === 1;
}
```

**Time Complexity**: O(log n). Similar logic applies as for the first method, but the cycle detection works in constant space which makes it faster in practice.  
**Space Complexity**: O(1), as no extra space is used to store previous numbers.

