# [739. Daily Temperatures](https://leetcode.com/problems/daily-temperatures/)

## Approach 1: Brute Force

### Solution
csharp
```csharp
// Time Complexity: O(n^2)
// Space Complexity: O(1)
public class Solution {
    public int[] DailyTemperatures(int[] temperatures) {
        int n = temperatures.Length;
        int[] result = new int[n];

        // Compare each day's temperature with the subsequent days
        for (int i = 0; i < n; i++) {
            for (int j = i + 1; j < n; j++) {
                if (temperatures[j] > temperatures[i]) {
                    result[i] = j - i; // Days until a warmer temperature is found
                    break;
                }
            }
        }

        return result;
    }
}
```

## Approach 2: Using a Stack (Optimal Solution)

### Solution
csharp
```csharp
// Time Complexity: O(n)
// Space Complexity: O(n)
using System.Collections.Generic;

public class Solution {
    public int[] DailyTemperatures(int[] temperatures) {
        int n = temperatures.Length;
        int[] result = new int[n];
        Stack<int> stack = new Stack<int>(); // Stores indices of temperatures

        // Traverse the temperatures array
        for (int i = 0; i < n; i++) {
            // Process indices in the stack with temperatures less than the current
            while (stack.Count > 0 && temperatures[i] > temperatures[stack.Peek()]) {
                int index = stack.Pop();
                result[index] = i - index; // Calculate the days until a warmer temperature
            }
            stack.Push(i); // Push the current day's index onto the stack
        }

        return result;
    }
}
```

## Approach 3: Reverse Traversal (Optimized for Space)

### Solution
csharp
```csharp
// Time Complexity: O(n)
// Space Complexity: O(1)
public class Solution {
    public int[] DailyTemperatures(int[] temperatures) {
        int n = temperatures.Length;
        int[] result = new int[n];
        int hottest = 0; // Track the hottest temperature seen from the right

        // Traverse the array in reverse
        for (int i = n - 1; i >= 0; i--) {
            if (temperatures[i] >= hottest) {
                result[i] = 0; // No warmer temperature in the future
                hottest = temperatures[i];
            } else {
                int days = 1;
                // Find the next warmer day using result array
                while (temperatures[i + days] <= temperatures[i]) {
                    days += result[i + days];
                }
                result[i] = days;
            }
        }

        return result;
    }
}
```

