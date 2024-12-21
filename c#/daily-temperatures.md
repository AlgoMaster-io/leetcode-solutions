## [Leetcode 739: Daily Temperatures](https://leetcode.com/problems/daily-temperatures/)

### Approach
- [Brute Force](#brute-force)
- [Optimized with Stack](#optimized-with-stack)

---

### Brute Force

#### Intuition
The brute force approach involves considering each temperature and finding the next warmer day by looking at each subsequent day. Though this approach is straightforward, it is not efficient for large inputs. We iterate over each temperature, then for each temperature, traverse the rest of the list looking for a warmer day. This results in an O(n^2) time complexity.

#### Steps
1. Initialize an array `result` with zeros, representing the number of days until a warmer temperature for each day.
2. Iterate over each day with a variable `i`.
3. For each day `i`, look for the next day `j` where `temperatures[j] > temperatures[i]`.
4. If such a day is found, set `result[i] = j - i`.
5. Return the `result` array.

#### Code
```csharp
public int[] DailyTemperatures(int[] temperatures) {
    int n = temperatures.Length;
    int[] result = new int[n];
    
    for (int i = 0; i < n; i++) {
        for (int j = i + 1; j < n; j++) {
            if (temperatures[j] > temperatures[i]) {
                result[i] = j - i;
                break; // Break once we find the next warmer day
            }
        }
    }
    
    return result;
}
```

#### Time and Space Complexity
- **Time Complexity**: O(n^2), where n is the number of temperatures. We are checking each element with every possible following element.
- **Space Complexity**: O(1), aside from the input and output structures.

---

### Optimized with Stack

#### Intuition
To improve upon the brute force solution, we can use a stack to keep track of the indices of the temperatures that are waiting for a warmer day. We iterate backwards through the temperatures and use the stack to efficiently find the next warmer day. This approach takes advantage of the fact that we can directly jump to the next warmer temperature by using the stack, eliminating the need to check each subsequent temperature.

#### Steps
1. Initialize an empty stack `stack` to keep track of indices of temperatures.
2. Initialize an array `result` with zeros, representing the count of days until the next warmer temperature for each day.
3. Iterate over the array of temperatures from right to left.
4. For each day `i`:
   - While the stack is not empty and the temperature at the top of the stack is less than or equal to the current temperature, pop the stack.
   - If the stack is not empty, the day at the top of the stack is the next warmer day, so set `result[i]` to the difference between stack top index and `i`.
   - Push the current index `i` onto the stack.
5. Return the `result` array.

#### Code
```csharp
public int[] DailyTemperatures(int[] temperatures) {
    int n = temperatures.Length;
    int[] result = new int[n];
    Stack<int> stack = new Stack<int>();
    
    for (int i = n - 1; i >= 0; i--) {
        // Pop the stack until we find a warmer temperature
        while (stack.Count > 0 && temperatures[stack.Peek()] <= temperatures[i]) {
            stack.Pop();
        }
        
        // If the stack is not empty, find the days until next warmer temperature
        if (stack.Count > 0) {
            result[i] = stack.Peek() - i;
        }
        
        // Push current index onto the stack
        stack.Push(i);
    }
    
    return result;
}
```

#### Time and Space Complexity
- **Time Complexity**: O(n), where n is the number of temperatures. Each temperature index is pushed and popped from the stack once.
- **Space Complexity**: O(n), needed for the stack to store indices.

