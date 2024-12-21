# [Leetcode 1944: Number of Visible People in a Queue](https://leetcode.com/problems/number-of-visible-people-in-a-queue/)

### Approaches
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Monotonic Decreasing Stack (Optimal)](#approach-2-monotonic-decreasing-stack-optimal)

## Approach 1: Brute Force

### Intuition
In this approach, we iterate over each person in the queue and for each person, we look to the right to count how many people they can see. A person can see another person if all the intermediate people are shorter, and the person they see doesn't exceed their height.

### Code
```typescript
function canSeePersonsCount(heights: number[]): number[] {
    const n = heights.length;
    const result = new Array(n).fill(0);

    // Iterate over each person at index `i`.
    for(let i = 0; i < n; i++) {
        // For each person, check how many people they can see to the right.
        for(let j = i + 1; j < n; j++) {
            // If the person at j is taller than the person at i,
            // increase the visibility count for person i.
            if(heights[j] >= heights[i]) {
                result[i]++;
                break;
            }
            // If the person at j is shorter, increase the visibility count
            result[i]++;
        }
    }
    
    return result;
}
```

### Time Complexity
- O(n^2), where n is the number of people in the queue. For each person, we may potentially iterate over all others to their right.

### Space Complexity
- O(n), where n is the space needed for the result array.

## Approach 2: Monotonic Decreasing Stack (Optimal)

### Intuition
The stack approach efficiently handles the determination of visibility by leveraging the concept of maintaining a stack to keep track of heights in a way that efficiently counts how many people can be seen. The idea is to maintain a stack where heights are in descending order from the top (closest) to the bottom (farthest).

1. Traverse the people list from right to left.
2. Use the stack to keep track of people that have been "seen" by current person.
3. The stack will automatically contain people that can be potentially seen directly because anyone taller coming after a shorter makes the shorter invisible to future elements.

### Code
```typescript
function canSeePersonsCount(heights: number[]): number[] {
    const n = heights.length;
    const result = new Array(n).fill(0);
    const stack: number[] = [];

    // Traverse from right to left
    for(let i = n - 1; i >= 0; i--) {
        // While the stack is not empty, and the current height is greater than or equal to
        // the height of the person currently on top of the stack, pop elements.
        while(stack.length && heights[i] > heights[stack[stack.length - 1]]) {
            stack.pop();
            result[i]++;  // We've popped one, so increment count for current person
        }
        // If the stack still contains an element after clearing smaller elements,
        // it means there's at least one person they can see who is taller.
        if(stack.length) {
            result[i]++;
        }
        // Push current index onto the stack
        stack.push(i);
    }
    
    return result;
}
```

### Time Complexity
- O(n), where n is the number of people in the queue. Every person is pushed and popped from the stack exactly once, leading to linear time complexity.

### Space Complexity
- O(n), the space for the result array and the worst-case scenario of having all people in the stack.

