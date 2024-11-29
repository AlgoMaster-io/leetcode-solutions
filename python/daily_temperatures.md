# [739. Daily Temperatures](https://leetcode.com/problems/daily-temperatures/)

## Approach 1: Brute Force

### Solution
python
```python
# Time Complexity: O(n^2)
# Space Complexity: O(1)
class Solution:
    def dailyTemperatures(self, temperatures):
        n = len(temperatures)
        result = [0] * n

        # Compare each day's temperature with the subsequent days
        for i in range(n):
            for j in range(i + 1, n):
                if temperatures[j] > temperatures[i]:
                    result[i] = j - i  # Days until a warmer temperature is found
                    break

        return result
```

## Approach 2: Using a Stack (Optimal Solution)

### Solution
python
```python
# Time Complexity: O(n)
# Space Complexity: O(n)
class Solution:
    def dailyTemperatures(self, temperatures):
        n = len(temperatures)
        result = [0] * n
        stack = []  # Stores indices of temperatures

        # Traverse the temperatures array
        for i in range(n):
            # Process indices in the stack with temperatures less than the current
            while stack and temperatures[i] > temperatures[stack[-1]]:
                index = stack.pop()
                result[index] = i - index  # Calculate the days until a warmer temperature
            stack.append(i)  # Push the current day's index onto the stack

        return result
```

## Approach 3: Reverse Traversal (Optimized for Space)

### Solution
python
```python
# Time Complexity: O(n)
# Space Complexity: O(1)
class Solution:
    def dailyTemperatures(self, temperatures):
        n = len(temperatures)
        result = [0] * n
        hottest = 0  # Track the hottest temperature seen from the right

        # Traverse the array in reverse
        for i in range(n - 1, -1, -1):
            if temperatures[i] >= hottest:
                result[i] = 0  # No warmer temperature in the future
                hottest = temperatures[i]
            else:
                days = 1
                # Find the next warmer day using result array
                while temperatures[i + days] <= temperatures[i]:
                    days += result[i + days]
                result[i] = days

        return result
```

