
# Happy Number
Write an algorithm to determine if a number n is a "happy number."

A happy number is defined by the following process:

- Starting with any positive integer, replace the number by the sum of the squares of its digits.
- Repeat the process until the number equals 1 (where it will stay), or it loops endlessly in a cycle that does not include 1.
- Those numbers for which this process ends in 1 are happy numbers.

### Constraints:
1 <= n <= 2^31 - 1

### Examples
```javascript
Input: n = 19
Output: true
Explanation: 
1² + 9² = 82
8² + 2² = 68
6² + 8² = 100
1² + 0² + 0² = 1

Input: n = 2
Output: false
```

## Approaches to Solve the Problem
### Approach 1: Hash Set to Track Seen Numbers
##### Intuition:
One way to detect a cycle is by keeping track of all numbers we've already seen. If we see the same number again, it means we are in a cycle, and the number is not happy. If the process eventually reaches 1, it is a happy number.

1. Initialize an empty set to store previously seen numbers.
2. Repeatedly compute the sum of the squares of the digits of the number.
3. If the sum equals 1, return True.
4. If the sum is already in the set, return False (indicating a cycle).
5. Add the sum to the set and repeat.
##### Time Complexity:
O(log n), where n is the input number. We break down the number into digits and repeatedly compute their squares.
##### Space Complexity:
O(log n), since we store previously seen numbers in a set, which can grow based on the number of digits of n.
##### Python Code:
```python
def isHappy(n: int) -> bool:
    def sum_of_squares(num):
        total = 0
        while num:
            digit = num % 10  # Extract the last digit
            total += digit ** 2  # Add square of the digit to total
            num //= 10  # Remove the last digit
        return total

    seen = set()  # Set to store numbers we have seen
    
    while n != 1 and n not in seen:
        seen.add(n)  # Add current number to the seen set
        n = sum_of_squares(n)  # Compute the sum of squares of digits
    
    return n == 1  # Return True if 1 is reached, else False
```
### Approach 2: Two Pointers (Floyd’s Cycle Detection Algorithm)
##### Intuition: 
A more efficient way to detect a cycle is by using two pointers—slow and fast. This is similar to Floyd's Cycle Detection algorithm (used for detecting cycles in linked lists). The slow pointer moves one step at a time (computing the sum of squares once), while the fast pointer moves two steps at a time. If there is a cycle, the two pointers will eventually meet. If the fast pointer reaches 1, then the number is happy.

1. Initialize two pointers slow and fast at n.
2. Move slow by one step (compute sum of squares once).
3. Move fast by two steps (compute sum of squares twice).
4. If slow and fast meet, there is a cycle (return False).
5. If fast reaches 1, return True.
##### Why This Works:
- If a cycle exists, the slow and fast pointers will meet within the cycle. If the number is happy, the fast pointer will eventually reach 1.
##### Time Complexity:
O(log n), since the sum of the squares of digits is computed repeatedly.
##### Space Complexity:
O(1), since only a constant amount of space is used for the pointers.
##### Visualization:
```rust
Example: n = 19

Initial: slow = 19, fast = 19

Step 1: slow = sum_of_squares(19) = 82
        fast = sum_of_squares(sum_of_squares(19)) = sum_of_squares(82) = 68

Step 2: slow = sum_of_squares(82) = 68
        fast = sum_of_squares(sum_of_squares(68)) = sum_of_squares(100) = 1

Since fast reached 1, the number is happy (return True).
```
##### Python Code:
```python
def isHappy(n: int) -> bool:
    def sum_of_squares(num):
        total = 0
        while num:
            digit = num % 10
            total += digit ** 2
            num //= 10
        return total

    slow = n
    fast = sum_of_squares(n)
    
    while fast != 1 and slow != fast:
        slow = sum_of_squares(slow)  # Slow pointer moves one step
        fast = sum_of_squares(sum_of_squares(fast))  # Fast pointer moves two steps
    
    return fast == 1  # If fast reaches 1, it's a happy number
```
### Approach 3: Recursive Approach
##### Intuition: 
We can also implement the solution recursively. At each step, compute the sum of squares of the digits and recursively check if this number is happy. To detect cycles, we use a set to keep track of previously seen numbers.

1. Create a recursive function to compute the sum of squares.
2. If the current number is 1, return True.
3. If the number is in the set, return False (cycle detected).
4. Add the number to the set and continue the process.
##### Time Complexity:
O(log n), since the number is broken down into its digits recursively.
##### Space Complexity:
O(log n), because of the recursion depth and the set used to track seen numbers.
##### Python Code:
```python
def isHappy(n: int, seen=None) -> bool:
    if seen is None:
        seen = set()  # Set to track seen numbers
    
    if n == 1:
        return True  # Base case: if n is 1, it's a happy number
    if n in seen:
        return False  # If n is already seen, it's a cycle
    
    seen.add(n)  # Add current number to the seen set
    sum_of_squares = sum(int(digit) ** 2 for digit in str(n))  # Compute sum of squares
    
    return isHappy(sum_of_squares, seen)  # Recurse with the new number
```
## Summary

| Approach                         | Time Complexity | Space Complexity |
|-----------------------------------|-----------------|------------------|
| Hash Set (Tracking Seen Numbers)   | O(log n)            | O(log n)             |
| Two Pointers (Floyd’s Algorithm)        | O(log n)            | O(1)             |
| Recursive Approach	        | O(logn)            | O(log n)             |

The Two Pointers (Floyd’s Algorithm) is the most optimal solution as it uses constant space and efficiently detects cycles.