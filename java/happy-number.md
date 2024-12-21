# [LeetCode Problem 202: Happy Number](https://leetcode.com/problems/happy-number/)

This problem involves determining whether a given number is a "happy number."

## Approaches
1. [Naive Approach: Cycle Detection using a Set](#naive-approach-cycle-detection-using-a-set)
2. [Floyd's Cycle Detection (Tortoise and Hare)](#floyd-cycle-detection-tortoise-and-hare)

---

### Naive Approach: Cycle Detection using a Set

The simplest approach is to detect cycles by using a set to track sums of squared digits that we've seen. If we encounter a sum that we've seen before, we know we're in a cycle and the number is not happy. Otherwise, if we reach the number 1, the number is happy.

#### Intuition

- Each number is transformed by replacing it with the sum of the squares of its digits.
- If a number becomes 1 after this series of transformations, it's a happy number.
- If we encounter a sum we've seen before and it's not 1, then we're in a loop and the number is not happy.

#### Steps

1. Initialize a set to keep track of the sums we've encountered.
2. Loop to repeatedly calculate the sum of the squares of digits.
3. If the sum is 1, return true.
4. If the sum is already in the set, return false (cycle detected).
5. Add the sum to the set and repeat.

#### Code

```java
public class Solution {
    public boolean isHappy(int n) {
        Set<Integer> seen = new HashSet<>();
        
        while (n != 1 && !seen.contains(n)) {
            seen.add(n);
            n = getNext(n);
        }
        
        return n == 1;
    }
    
    private int getNext(int n) {
        int totalSum = 0;
        while (n > 0) {
            int digit = n % 10;
            totalSum += digit * digit;
            n /= 10;
        }
        return totalSum;
    }
}
```

#### Time and Space Complexity

- **Time Complexity**: O(log n). In the worst case, each number leads to a value with fewer digits.
- **Space Complexity**: O(log n). We store each intermediate value in a set until we reach 1 or encounter a cycle.

---

### Floyd's Cycle Detection (Tortoise and Hare)

A more space-optimized solution is to utilize Floyd's Cycle Detection Algorithm, known for its tortoise and hare pointers, which operates in constant space.

#### Intuition

- We use two pointers: a slow-moving "tortoise" and a fast-moving "hare."
- The tortoise moves one step at a time, while the hare moves two steps.
- If there is a cycle, they will eventually meet.
- If there is no cycle, one of them will reach 1, confirming that the number is happy.

#### Steps

1. Initialize slow and fast pointers both to the original number.
2. Compute the next number for the slow pointer (one step) and for the fast pointer (two steps).
3. If fast pointer reaches 1, return true.
4. If slow equals fast and they are not 1, a cycle is detected, return false.
5. Repeat these steps until a conclusion is reached.

#### Code

```java
public class Solution {
    public boolean isHappy(int n) {
        int slow = n;
        int fast = getNext(n);
        
        while (fast != 1 && slow != fast) {
            slow = getNext(slow);              // Move one step
            fast = getNext(getNext(fast));     // Move two steps
        }
        
        return fast == 1;
    }
    
    private int getNext(int n) {
        int totalSum = 0;
        while (n > 0) {
            int digit = n % 10;
            totalSum += digit * digit;
            n /= 10;
        }
        return totalSum;
    }
}
```

#### Time and Space Complexity

- **Time Complexity**: O(log n). Similar to the set-based approach, but faster in practice due to constant space usage.
- **Space Complexity**: O(1). Only uses a fixed amount of extra space for pointers and variables; no need for additional data structures.

---

These approaches provide a comprehensive view from a basic cycle-detection method to an optimized approach using cycle-detection algorithms. Both achieve the goal, but using Floyd's Cycle Detection offers better space efficiency.

