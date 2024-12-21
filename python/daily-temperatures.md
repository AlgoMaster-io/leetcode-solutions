## [Daily Temperatures - Leetcode 739](https://leetcode.com/problems/daily-temperatures/)

### Approaches:
1. [Brute Force](#brute-force)
2. [Stack Based Solution](#stack-based-solution)

### Brute Force

#### Intuition:
The simplest way to solve this problem is to use a brute-force approach where, for each day, we iterate over the subsequent days to find out when the next warmer temperature occurs.

#### Implementation:

```python
def dailyTemperatures(temperatures):
    n = len(temperatures)  # Length of the temperatures list
    answer = [0] * n  # Initialize the answer with all zeros
    
    for i in range(n):  # Iterate over each day
        for j in range(i + 1, n):  # Check subsequent days
            if temperatures[j] > temperatures[i]:  # If a warmer temperature is found
                answer[i] = j - i  # Calculate the days difference
                break  # Break the inner loop since we've found the day we needed
                
    return answer
```

#### Time Complexity:
- **O(n^2)** where `n` is the number of days, because for each day we might have to check all subsequent days.

#### Space Complexity:
- **O(1)** extra space, not including the space needed for the output.

### Stack Based Solution

#### Intuition:
The stack-based approach uses a monotonic decreasing stack to keep track of temperatures. For each day, we pop from the stack until we find a day with a higher temperature or the stack becomes empty. This approach leverages the Last-In-First-Out nature of stacks to efficiently find the next warmer day.

#### Implementation:

```python
def dailyTemperatures(temperatures):
    n = len(temperatures)  # Length of the temperatures list
    answer = [0] * n  # Initialize the answer with all zeros
    stack = []  # This stack will store indices of the temperatures
    
    for current_day in range(n):
        current_temp = temperatures[current_day]  # Get the temperature for the current day
        
        # While the stack is not empty and the current day's temperature is greater
        # than the temperature at the top index of the stack
        while stack and temperatures[stack[-1]] < current_temp:
            previous_day = stack.pop()  # Get the index of the previous day that was cooler
            answer[previous_day] = current_day - previous_day  # Calculate the days difference
            
        # Push the current day's index onto the stack
        stack.append(current_day)
        
    return answer
```

#### Time Complexity:
- **O(n)**, because each index is pushed and popped from the stack at most once.

#### Space Complexity:
- **O(n)** in the worst case, due to the stack storing all indices when temperatures are strictly decreasing.

