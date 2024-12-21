# [1944. Number of Visible People in a Queue](https://leetcode.com/problems/number-of-visible-people-in-a-queue/)

## Approaches

1. [Brute Force Approach](#brute-force-approach)
2. [Optimized Stack Approach](#optimized-stack-approach)

### Brute Force Approach

The naive approach to solving this problem is to simulate the process of looking over the heads in the queue step by step.

- For each person in the queue, iterate over the people in front of them.
- Keep track of how many people can be seen until a taller person is found.
- Due to nested iterations, this approach is not optimal but serves as a good baseline.

#### Intuition

- We start from each person and look towards the end of the queue.
- For a person to be visible, they must be taller than all preceding visible ones.

#### Time and Space Complexity

- **Time Complexity:** O(n^2), where n is the number of people in the queue.
- **Space Complexity:** O(n), for the result array.

```javascript
function canSeePersonsCount(heights) {
    const n = heights.length;
    const result = new Array(n).fill(0);

    // Iterate for each person in given queue
    for (let i = 0; i < n; i++) {
        // Check subsequent people in queue
        for (let j = i + 1; j < n; j++) {
            // If next person is higher, break as they block further view
            if (heights[j] >= heights[i]) {
                result[i]++;
                break;
            } else {
                // Otherwise, just increment the visible count
                result[i]++;
            }
        }
    }
    return result;
}
```

### Optimized Stack Approach

Instead of checking every person for each individual, we can use a stack data structure to efficiently track the heights that are still relevant. This approach greatly reduces the amount of unnecessary computation.

#### Intuition

- We traverse the queue backwards and for each person, we use a stack to track the people they can see.
- The stack helps us find the first taller person in front of them.

#### Steps:

1. Traverse the list from the end to the beginning.
2. Use a stack to store indices of people who can still be "seen."
3. For each person, pop everyone from the stack who is shorter because they don't block the view.
4. The size of the stack immediately before a taller element represents the number of visible people.

#### Time and Space Complexity

- **Time Complexity:** O(n), since each person is pushed and popped from the stack at most once.
- **Space Complexity:** O(n), for the stack.

```javascript
function canSeePersonsCount(heights) {
    const n = heights.length;
    const result = new Array(n).fill(0);
    const stack = [];

    // Traverse the queue from the end
    for (let i = n - 1; i >= 0; i--) {
        // Count of visible people
        let visibleCount = 0;

        // As long as there are people on the stack and they're shorter, pop them
        while (stack.length > 0 && stack[stack.length - 1] < heights[i]) {
            stack.pop();
            visibleCount++;
        }

        // If there's still someone in the stack, it means they're the first taller person
        if (stack.length > 0) {
            visibleCount++;
        }

        // Store result for current height
        result[i] = visibleCount;

        // Push current person to the stack
        stack.push(heights[i]);
    }
    return result;
}
```

This optimized stack approach provides a clear, linear-time solution, ensuring efficient calculation of visible people in a queue.

