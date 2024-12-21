# [Leetcode 202: Happy Number](https://leetcode.com/problems/happy-number/)

## Approaches:
- [Approach 1: Cycle Detection using HashSet](#approach-1-cycle-detection-using-hashset)
- [Approach 2: Floyd's Cycle Detection Algorithm (Tortoise and Hare)](#approach-2-floyds-cycle-detection-algorithm-tortoise-and-hare)

## Approach 1: Cycle Detection using HashSet

### Intuition
The idea is to use a `HashSet` to detect cycles in the sequence of sums. If a number is happy, its sequence will eventually reach `1`. If a cycle forms, it means the sequence will never reach `1`, and the number is not happy. By storing already seen sums in a set, we can determine if a new sum has been encountered before.

### Steps:
1. Calculate the sum of the squares of the digits of the number.
2. Check if the sum is 1, if so, the number is happy.
3. If the sum is already in the set, a cycle is detected, and the number is not happy.
4. Add the sum to the set and repeat steps 1-3.

### C++ Code

```cpp
#include <iostream>
#include <unordered_set>
using namespace std;

bool isHappy(int n) {
    unordered_set<int> seen; // Set to store seen numbers
    
    while (n != 1 && seen.find(n) == seen.end()) {
        seen.insert(n); // Add current number to set
        int current = n;
        int sum = 0;

        // Calculate the sum of the squares of digits
        while (current != 0) {
            int digit = current % 10;
            sum += digit * digit;
            current /= 10;
        }
        
        n = sum; // Update n to the sum for next iteration
    }
    
    // If n is 1, it is a happy number; otherwise, it's not
    return n == 1;
}
```
### Complexity Analysis
- **Time Complexity:** `O(log n)` on average as we reduce the number of digits each time.
- **Space Complexity:** `O(log n)` due to storing previous sums in the set.

## Approach 2: Floyd's Cycle Detection Algorithm (Tortoise and Hare)

### Intuition
This approach uses the Floyd Cycle Detection method which is an optimal cycle detection technique using two pointers, also known as the "Tortoise and the Hare" algorithm. It efficiently detects cycles by using two pointers moving at different speeds.

### Steps:
1. Use two pointers: slow (`tortoise`) and fast (`hare`).
2. Move tortoise at normal speed (one step) and hare at double speed (two steps).
3. Calculate the sum of squares for each move.
4. If the hare meets tortoise (both pointers equal), a cycle exists, and the number is not happy.
5. If either pointer reaches 1, the number is happy.

### C++ Code

```cpp
#include <iostream>
using namespace std;

int getNextNumber(int n) {
    int sum = 0;
    while (n > 0) {
        int digit = n % 10;
        sum += digit * digit;
        n /= 10;
    }
    return sum;
}

bool isHappy(int n) {
    int slow = n; // Initialize slow pointer
    int fast = getNextNumber(n); // Initialize fast pointer

    // Loop until fast reaches 1 or both pointers meet
    while (fast != 1 && slow != fast) {
        slow = getNextNumber(slow); // Move slow one step
        fast = getNextNumber(getNextNumber(fast)); // Move fast two steps
    }

    // Successful if fast reached 1
    return fast == 1;
}
```

### Complexity Analysis
- **Time Complexity:** `O(log n)`; similar logic of reducing digits applies here as well.
- **Space Complexity:** `O(1)`; no additional space for storing the sequence.

Both approaches efficiently solve the happy number problem, with Floyd's cycle detection being more optimal concerning space complexity due to its constant space requirement.

