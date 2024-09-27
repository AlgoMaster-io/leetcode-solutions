# Daily Temperatures
Given a list of daily temperatures temperatures, return a list answer such that for each day in the input, answer[i] is the number of days you would have to wait until a warmer temperature. If there is no future day for which this is possible, put 0 instead.

### Constraints:
- 1 <= temperatures.length <= 10^5
- 30 <= temperatures[i] <= 100

### Examples
```javascript
Input: temperatures = [73,74,75,71,69,72,76,73]
Output: [1,1,4,2,1,1,0,0]

Input: temperatures = [30,40,50,60]
Output: [1,1,1,0]

Input: temperatures = [30,60,90]
Output: [1,1,0]
```

## Approaches to Solve the Problem
### Approach 1: Brute Force (Inefficient)
##### Intuition:
The brute-force approach involves checking every temperature for each day to find the next warmer day. For each day i, scan the remaining days to find the first day where the temperature is higher.

Steps:
1. For each day i, start a nested loop to search through the following days j > i.
2. If a day j is found where temperatures[j] > temperatures[i], calculate the difference j - i and store it in the result.
3. If no warmer day is found, store 0 for that day.
##### Time Complexity:
O(n²), where n is the number of days. For each day, we search all the following days, making this approach inefficient for large inputs.
##### Space Complexity:
O(n) for storing the result array.
##### Python Code:
```python
def dailyTemperatures(temperatures):
    n = len(temperatures)
    answer = [0] * n
    
    for i in range(n):
        for j in range(i + 1, n):
            if temperatures[j] > temperatures[i]:
                answer[i] = j - i
                break
    
    return answer
```

### Approach 2: Stack (Optimal Solution)
##### Intuition: 
A more efficient solution can be achieved using a monotonic decreasing stack. The idea is to traverse the array from left to right while maintaining a stack of indices of the temperatures. The stack helps keep track of temperatures that we haven't found a warmer day for yet. When we find a warmer temperature, we calculate the difference in days and store the result.

- The stack stores indices of temperatures in decreasing order.
- When a warmer temperature is found (i.e., temperatures[i] > temperatures[stack[-1]]), we pop the stack, calculate the difference, and store it in the result array.

Steps:
1. Initialize an empty stack and an array answer filled with 0's.
2. Traverse the array from left to right.
3. For each day i, check if the current temperature is higher than the temperature at the index stored in the stack.
   - If true, pop the index from the stack, calculate i - stack[-1] as the number of days until a warmer temperature, and update the result.
   - Repeat until the stack is empty or the temperature at the top of the stack is higher.
4. Push the current index i onto the stack.
5. Continue until the array is fully processed.
### Visualization
For temperatures = [73, 74, 75, 71, 69, 72, 76, 73]:

```rust
Start with empty stack.

Step 1: 
Index 0 (73): No warmer day yet → push 0 onto the stack.
Stack: [0]

Step 2: 
Index 1 (74): Warmer than 73 → pop 0, set answer[0] = 1 → push 1 onto the stack.
Stack: [1]

Step 3: 
Index 2 (75): Warmer than 74 → pop 1, set answer[1] = 1 → push 2 onto the stack.
Stack: [2]

...

Final result: [1, 1, 4, 2, 1, 1, 0, 0]
```
##### Time Complexity:
O(n), where n is the number of days. Each index is pushed and popped from the stack at most once.
##### Space Complexity:
O(n), for the stack and the result array.
##### Python Code:
```python
def dailyTemperatures(temperatures):
    n = len(temperatures)
    answer = [0] * n
    stack = []
    
    for i, temp in enumerate(temperatures):
        # While the stack is not empty and we have found a warmer temperature
        while stack and temperatures[stack[-1]] < temp:
            prev_index = stack.pop()
            answer[prev_index] = i - prev_index
        
        # Push the current index onto the stack
        stack.append(i)
    
    return answer
```
### Edge Cases:
1. Single Element: If the input array has only one element, the result should be [0], since there are no future days.
2. All Decreasing Temperatures: If the temperatures are in strictly decreasing order, the result will be all 0's because there is no warmer day for any of the elements.
3. All Increasing Temperatures: The result will be [1, 1, 1, ..., 0] since each day has the next day as a warmer day.
## Summary

| Approach                         | Time Complexity | Space Complexity |
|-----------------------------------|-----------------|------------------|
| Brute Force                        | O(n²)      | O(n)             |
| Stack (Optimal Solution)                          | O(n)            | O(n)             |

The Stack approach is the most efficient solution, leveraging a monotonic stack to keep track of indices and efficiently calculate the number of days until a warmer temperature. This solution runs in linear time and is optimal for the input size constraints.