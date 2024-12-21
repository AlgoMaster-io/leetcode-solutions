# [LeetCode Problem 202: Happy Number](https://leetcode.com/problems/happy-number/)

## Table of Contents
1. [Basic Approach](#basic)
2. [Optimal Approach](#optimal)

### Basic Approach: HashSet to Detect Cycle

#### Intuition:
A happy number is defined as a number that eventually reaches 1 when you repeatedly replace it with the sum of the squares of its digits. If a number is not happy, it will eventually enter a cycle that does not include 1. A straightforward way to detect cycles is to use a data structure that can track whether we've seen a certain state (sum of digit squares) before, such as a HashSet.

#### Approach:
1. Initialize an empty hash set to store seen numbers.
2. Use a loop to continually replace the number with the sum of the squares of its digits.
3. During each iteration, check if the current number is in the hash set:
   - If it is, a cycle is detected, and the number is not a happy number since we are looping over the cycle.
   - If it is 1, the number is happy.
   - Otherwise, add this number to the set and proceed to the next iteration.

#### Code:
```csharp
public class Solution {
    public bool IsHappy(int n) {
        HashSet<int> seen = new HashSet<int>();
        while (n != 1 && !seen.Contains(n)) {
            seen.Add(n); // Add current number to seen set to detect cycles
            n = GetNext(n); // Generate the next number in the sequence
        }
        return n == 1; // If it breaks out of the loop because n == 1, it's a happy number
    }

    private int GetNext(int n) {
        int sum = 0;
        while (n > 0) { // Calculate sum of squares of digits
            int digit = n % 10;
            sum += digit * digit; // Square the digit and add to sum
            n /= 10; // Remove last digit from n
        }
        return sum;
    }
}
```

#### Time Complexity: 
- Each computation of `GetNext(n)` takes O(log n) time.
- The number of iterations is, in theory, limited by the cycle length of unhappy numbers, hence often approximated as constant or O(1).
  
#### Space Complexity:
- O(log n): The hash set can potentially keep numbers up to the size of log(n) digits.

### Optimal Approach: Floyd's Cycle Finding Algorithm

#### Intuition:
Instead of using extra space to keep track of numbers in the sequence, we can use the two-pointer technique (slow and fast) to detect cycles. This is commonly known as Floyd's Cycle Finding Algorithm and can efficiently determine the presence of cycles.

#### Approach:
1. Initialize two pointers, slow and fast.
2. Move slow one step at a time and fast two steps at a time.
3. If there is a cycle, slow and fast pointers will eventually meet at the same number.
4. If the number is happy, one of the pointers will reach 1.

#### Code:
```csharp
public class Solution {
    public bool IsHappy(int n) {
        int slow = n;
        int fast = GetNext(n);

        while (fast != 1 && slow != fast) { // Continue until fast reaches 1 or both pointers meet in a cycle
            slow = GetNext(slow); // Move slow by 1 step
            fast = GetNext(GetNext(fast)); // Move fast by 2 steps
        }

        return fast == 1; // If fast becomes 1, then we've found a happy number
    }

    private int GetNext(int n) {
        int sum = 0;
        while (n > 0) {
            int digit = n % 10;
            sum += digit * digit;
            n /= 10;
        }
        return sum;
    }
}
```

#### Time Complexity:
- Similar logic as before, with O(log n) operations per `GetNext(n)` and usually constant iterations due to cycle length.

#### Space Complexity:
- O(1): No additional space required as we don't use auxiliary data structures, merely pointers.

