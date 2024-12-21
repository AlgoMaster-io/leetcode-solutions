# [Leetcode Problem 739: Daily Temperatures](https://leetcode.com/problems/daily-temperatures/)

## Approaches:
- [Brute Force Approach](#brute-force-approach)
- [Optimal Stack Approach](#optimal-stack-approach)

## Brute Force Approach

### Intuition:
The brute force approach involves a simple nested loop where for each temperature, the algorithm checks subsequent days until a warmer day is found. This is an intuitive and straightforward method but can be inefficient, especially for larger input arrays.

### Algorithm:
1. Initialize a result array with all zeros, since if no warmer day exists, we should record 0 for that day.
2. For each day `i`, iterate through the following days `j`:
   - If `temperatures[j]` is greater than `temperatures[i]`, calculate the difference `(j - i)` and store it in `result[i]`.
   - Break the loop since the closest warmer day is found.
3. Continue until the last day.

### Code:
```javascript
function dailyTemperatures(T) {
    // Create a result array initialized with zeros
    const result = new Array(T.length).fill(0);

    // Outer loop goes through each temperature
    for (let i = 0; i < T.length; i++) {
        // Inner loop checks successive temperatures
        for (let j = i + 1; j < T.length; j++) {
            // If a warmer temperature is found
            if (T[j] > T[i]) {
                // Calculate the number of days from i to j
                result[i] = j - i;
                // No need to check further, break the loop
                break;
            }
        }
    }
    return result;
}
```

### Time Complexity:
- **O(n^2)**, where `n` is the number of temperatures. Each temperature may require a check against all subsequent ones.

### Space Complexity:
- **O(1)**, apart from the output array, we use a constant amount of extra space.

## Optimal Stack Approach

### Intuition:
A more optimal approach utilizes a stack to keep track of indices that have yet to find a warmer temperature. This approach allows us to efficiently skip over days that are colder or equal and quickly find the next warmer day once it is encountered.

### Algorithm:
1. Initialize a stack to store indices of temperatures.
2. Iterate over each temperature:
   - While the stack is not empty and the current temperature is greater than the temperature at the index stored at the top of the stack:
     - This means the current day is a warmer day for the index at the top of the stack.
     - Pop the index from the stack and calculate the difference in days, storing it in the result array.
   - Push the current index onto the stack.
3. Continue until the end, any indices left in the stack imply there are no warmer days in the future.
   
### Code:
```javascript
function dailyTemperatures(T) {
    // Create a result array initialized with zeros
    const result = new Array(T.length).fill(0);
    const stack = [];

    // Iterate through each temperature
    for (let i = 0; i < T.length; i++) {
        // While there's a warmer temperature, process the stack
        while (stack.length > 0 && T[i] > T[stack[stack.length - 1]]) {
            // Get the index of the colder temperature
            const colderIndex = stack.pop();
            // Calculate the number of days until a warmer temperature
            result[colderIndex] = i - colderIndex;
        }
        // Push the current day's index onto the stack
        stack.push(i);
    }

    return result;
}
```

### Time Complexity:
- **O(n)**, each temperature is pushed and popped from the stack at most once.

### Space Complexity:
- **O(n)**, for the stack storing the indices.

In conclusion, the stack approach provides a more efficient solution, reducing the time complexity from quadratic to linear by avoiding unnecessary comparisons.

