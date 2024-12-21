# [Leetcode 1944: Number of Visible People in a Queue](https://leetcode.com/problems/number-of-visible-people-in-a-queue/)

## Approaches

- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Monotonic Stack](#approach-2-monotonic-stack)

### Approach 1: Brute Force

#### Intuition

The brute force approach is straightforward. For each person in the queue, we iterate to the right to check how many people are visible. A person is only visible if no person taller than them is between the current person and that visible person.

#### Code

```cpp
#include <iostream>
#include <vector>

using namespace std;

vector<int> canSeePersonsCount(vector<int>& heights) {
    int n = heights.size();
    vector<int> result(n, 0);
    
    // Iterate through each person in the queue
    for (int i = 0; i < n; ++i) {
        // Count visible people from current position i
        for (int j = i + 1; j < n; ++j) {
            // A person is visible if there is no taller person before them
            if (heights[j] >= heights[i]) {
                result[i]++;
                break;
            }
            result[i]++;
        }
    }
    
    return result;
}
```

#### Time Complexity

- **Time Complexity:** O(n^2)
  - For each person, we may need to compare them with every person to their right.
  
- **Space Complexity:** O(1)
  - No additional space is used apart from the input and output arrays.

### Approach 2: Monotonic Stack

#### Intuition

The core idea is to use a decreasing monotonic stack to efficiently determine for each person how many people they can see. By processing from right to left, we can track visible people in constant time.

1. Traverse the queue from right to left, maintaining a stack:
   - If the current person is taller than stack top, they can see the person at stack top, continue popping until this is no longer true.
   - Push the current person onto the stack.

#### Code

```cpp
#include <iostream>
#include <vector>
#include <stack>

using namespace std;

vector<int> canSeePersonsCount(vector<int>& heights) {
    int n = heights.size();
    vector<int> result(n, 0);
    stack<int> stk;
    
    // Traverse the queue from right to left
    for (int i = n - 1; i >= 0; --i) {
        // Count people visible and maintain the order of decreasing heights
        int visibleCount = 0;
        while (!stk.empty() && stk.top() < heights[i]) {
            ++visibleCount; // Pop and count visible people
            stk.pop();
        }
        
        // Store the result
        result[i] = visibleCount + (stk.empty() ? 0 : 1);
        
        // Push current person onto stack
        stk.push(heights[i]);
    }
    
    return result;
}
```

#### Time Complexity

- **Time Complexity:** O(n)
  - Each element is pushed and popped from the stack once.
  
- **Space Complexity:** O(n)
  - Stack is used to store elements temporarily.

In conclusion, using a monotonic stack significantly optimizes the solution by efficiently accounting for visible people with respect to the height constraints in constant time per person.

