# [739. Daily Temperatures](https://leetcode.com/problems/daily-temperatures/)

## Approach 1: Brute Force

### Solution
typescript
```typescript
// Time Complexity: O(n^2)
// Space Complexity: O(1)
function dailyTemperatures(temperatures: number[]): number[] {
    const n = temperatures.length;
    const result = new Array(n).fill(0);

    // Compare each day's temperature with the subsequent days
    for (let i = 0; i < n; i++) {
        for (let j = i + 1; j < n; j++) {
            if (temperatures[j] > temperatures[i]) {
                result[i] = j - i; // Days until a warmer temperature is found
                break;
            }
        }
    }

    return result;
}
```

## Approach 2: Using a Stack (Optimal Solution)

### Solution
typescript
```typescript
// Time Complexity: O(n)
// Space Complexity: O(n)
function dailyTemperatures(temperatures: number[]): number[] {
    const n = temperatures.length;
    const result = new Array(n).fill(0);
    const stack: number[] = []; // Stores indices of temperatures

    // Traverse the temperatures array
    for (let i = 0; i < n; i++) {
        // Process indices in the stack with temperatures less than the current
        while (stack.length > 0 && temperatures[i] > temperatures[stack[stack.length - 1]]) {
            const index = stack.pop()!;
            result[index] = i - index; // Calculate the days until a warmer temperature
        }
        stack.push(i); // Push the current day's index onto the stack
    }

    return result;
}
```

## Approach 3: Reverse Traversal (Optimized for Space)

### Solution
typescript
```typescript
// Time Complexity: O(n)
// Space Complexity: O(1)
function dailyTemperatures(temperatures: number[]): number[] {
    const n = temperatures.length;
    const result = new Array(n).fill(0);
    let hottest = 0; // Track the hottest temperature seen from the right

    // Traverse the array in reverse
    for (let i = n - 1; i >= 0; i--) {
        if (temperatures[i] >= hottest) {
            result[i] = 0; // No warmer temperature in the future
            hottest = temperatures[i];
        } else {
            let days = 1;
            // Find the next warmer day using result array
            while (temperatures[i + days] <= temperatures[i]) {
                days += result[i + days];
            }
            result[i] = days;
        }
    }

    return result;
}
```

