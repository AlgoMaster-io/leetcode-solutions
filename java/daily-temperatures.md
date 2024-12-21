# [739. Daily Temperatures](https://leetcode.com/problems/daily-temperatures/)

## Approaches
- [Brute Force Approach](#brute-force-approach)
- [Optimized Approach using Stack](#optimized-approach-using-stack)

---

### Brute Force Approach

**Intuition**:  
We can start with a straightforward approach by simply checking each temperature against all the following temperatures to see when a warmer day occurs. This involves traversing every pair of days, which can be inefficient.

#### Detailed Explanation:
1. For each day in the list of temperatures, we need to find the number of days until a warmer temperature appears.
2. We'll make a nested iteration, fixing one element from the start and iterating through the following temperatures to find the first one greater than our fixed element.
3. If we find such a day, we calculate how many days ahead it is and store that in our result list.

```java
public int[] dailyTemperaturesBruteForce(int[] T) {
    int n = T.length;
    int[] result = new int[n];
    
    for (int i = 0; i < n; i++) {
        // Check each following day for a warmer temperature
        for (int j = i + 1; j < n; j++) {
            if (T[j] > T[i]) {
                result[i] = j - i;
                break;
            }
        }
        // If no warmer day found, result[i] stays 0
    }
    
    return result;
}
```

**Time Complexity**: O(n^2)  
**Space Complexity**: O(1), aside from the output array.

---

### Optimized Approach using Stack

**Intuition**:  
By utilizing a stack, we can reduce the repeated comparisons to gain an optimal solution. We will use a stack to keep track of indices of the temperatures that need to find a warmer temperature.

#### Detailed Explanation:
1. We will maintain a stack to store indices of the temperatures.
2. As we iterate over each day's temperature, we compare it with the temperatures corresponding to the indices in our stack.
3. If the current day's temperature is warmer than the temperature at the index stored at the top of the stack, we pop the index from the stack and update our result with the number of days we've moved forward to find a warmer temperature.
4. We push the current index onto the stack if we don't find a warmer temperature.
5. This way, each temperature is processed once, ensuring efficiency.

```java
public int[] dailyTemperaturesStack(int[] T) {
    int n = T.length;
    int[] result = new int[n];
    Stack<Integer> stack = new Stack<>();
    
    for (int i = 0; i < n; i++) {
        // Check if the stack is not empty and the current temperature is greater than that at stack's top index
        while (!stack.isEmpty() && T[i] > T[stack.peek()]) {
            int index = stack.pop();
            result[index] = i - index; // The difference in days
        }
        // Push the current index onto the stack
        stack.push(i);
    }
    
    return result;
}
```

**Time Complexity**: O(n), each element is pushed and popped at most once.  
**Space Complexity**: O(n), due to the stack storing up to n indices. 

This stack-based solution significantly reduces the time complexity by ensuring each operation is amortized over the entire array, making it optimal for handling large datasets of daily temperatures.

