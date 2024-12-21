# [Leetcode 739: Daily Temperatures](https://leetcode.com/problems/daily-temperatures/)

## Approaches
- [Brute Force](#brute-force)
- [Optimal Stack-Based Solution](#optimal-stack-based-solution)

### Brute Force

The most straightforward solution to this problem is to use a brute force approach. For each day's temperature, traverse the rest of the array to find a warmer temperature or determine that there isn't one. This method is simple to implement but inefficient as it checks each pair of temperatures.

```typescript
function dailyTemperaturesBruteForce(T: number[]): number[] {
    const result: number[] = Array(T.length).fill(0);

    // Iterate over each day
    for (let i = 0; i < T.length; i++) {
        // Check for a warmer temperature in the subsequent days
        for (let j = i + 1; j < T.length; j++) {
            // If a warmer temperature is found
            if (T[j] > T[i]) {
                result[i] = j - i;  // Calculate days until warmer
                break;              // Exit the loop once found
            }
        }
    }
    
    return result;
}
```

**Time Complexity:** O(n^2) — For each element, you potentially have to search through the rest of the list.  
**Space Complexity:** O(n) — The output array takes up O(n) space.

### Optimal Stack-Based Solution

A more efficient solution uses a decreasing stack to keep track of indices with temperatures not yet resolved. As we traverse the temperature list, we use this stack to efficiently determine the next day's warmer temperature.

Intuition:
- We maintain a stack of indices such that each index maps to a temperature that is waiting for a warmer day.
- As we iterate through the temperature list, if the current temperature is higher than the temperature at the index on the top of the stack, it means we've found a warmer day for that index. We pop the index from the stack and calculate the difference in days.
- This ensures each index is pushed and popped only once, leading to an O(n) solution.

```typescript
function dailyTemperaturesStack(T: number[]): number[] {
    const result: number[] = Array(T.length).fill(0);
    const stack: number[] = [];  // Stack to store indices of temperatures

    // Traverse the temperature list
    for (let i = 0; i < T.length; i++) {
        // Check if the current temperature is higher than the temperature at the index stored at the top of the stack
        while (stack.length > 0 && T[i] > T[stack[stack.length - 1]]) {
            const index = stack.pop()!;  // Get the index of the day waiting for a warmer temperature
            result[index] = i - index;   // Calculate the number of days until a warmer temperature
        }
        // Push the index of the current day's temperature onto the stack
        stack.push(i);
    }
    
    return result;
}
```

**Time Complexity:** O(n) — Each element is pushed and popped from the stack exactly once.  
**Space Complexity:** O(n) — The stack stores indices, which in the worst case could be all indices in decreasing order.

