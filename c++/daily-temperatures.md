# [739. Daily Temperatures](https://leetcode.com/problems/daily-temperatures/)

## Approaches:
- [Brute Force Approach](#brute-force-approach)
- [Monotonic Stack Approach](#monotonic-stack-approach)

---

### Brute Force Approach

**Intuition**:  
The brute force approach is straightforward; for each day, we check the upcoming days to find a warmer temperature and calculate the number of days until that temperature occurs. Though easy to implement, this approach is inefficient for large inputs due to its time complexity.

**Approach**:
1. Initialize a result vector `result` of the same length as the input `temperatures`, and fill it with zeros.
2. Iterate over each element in the `temperatures` array with an index `i`.
3. For each element, iterate over the subsequent elements with an index `j` starting from `i+1`.
4. If a temperature `temperatures[j]` is found which is greater than `temperatures[i]`, update `result[i]` with the number of days `j-i`.
5. If no greater temperature is found till the end of the list, keep `result[i]` as zero.
6. Return the result vector.

```cpp
#include <vector>
using namespace std;

vector<int> dailyTemperatures(vector<int>& temperatures) {
    int n = temperatures.size();
    vector<int> result(n, 0);
    
    // Iterate through each day's temperature
    for (int i = 0; i < n; ++i) {
        // Check the subsequent days for a warmer temperature
        for (int j = i + 1; j < n; ++j) {
            if (temperatures[j] > temperatures[i]) {
                // Update the result with the number of days until a warmer temperature
                result[i] = j - i;
                break; // Exit the loop once the first warmer day is found
            }
        }
    }
    return result;
}
```

- **Time Complexity**: O(n^2), where n is the number of days, since for each day we potentially iterate through all subsequent days.
- **Space Complexity**: O(1), if we exclude the space used for the output.

---

### Monotonic Stack Approach

**Intuition**:  
To optimize the problem, we utilize a monotonically decreasing stack that keeps track of the indices of unresolved temperatures. As we iterate through the temperatures, we determine when a warmer temperature is encountered using the stack, which helps resolve multiple indices efficiently.

**Approach**:
1. Initialize a result vector `result` of the same length as `temperatures`, filled with zeros.
2. Use a stack to keep indices of temperatures.
3. Iterate through each day `i` in the `temperatures` array.
4. While the stack is not empty and the current day's temperature `temperatures[i]` is higher than the temperature at the index stored at the top of the stack, pop the stack. For each popped element, update `result` with the difference `i - stack.top()`.
5. Push the current day's index `i` onto the stack.
6. Continue this process until all temperatures are processed, then return the result vector.

```cpp
#include <vector>
#include <stack>
using namespace std;

vector<int> dailyTemperatures(vector<int>& temperatures) {
    int n = temperatures.size();
    vector<int> result(n, 0);
    stack<int> indices; // Stack to store unresolved indices
    
    for (int i = 0; i < n; ++i) {
        // Process the stack as long as there are unresolved temperatures
        // and a warmer current temperature is found.
        while (!indices.empty() && temperatures[i] > temperatures[indices.top()]) {
            int idx = indices.top();
            indices.pop();
            // Calculate the number of days until a warmer temperature
            result[idx] = i - idx;
        }
        // Push current index onto the stack
        indices.push(i);
    }
    
    return result;
}
```

- **Time Complexity**: O(n), where n is the number of days. Each index is pushed and popped from the stack at most once.
- **Space Complexity**: O(n), used for the stack to store indices.

