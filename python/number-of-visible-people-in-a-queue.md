## [Leetcode 1944: Number of Visible People in a Queue](https://leetcode.com/problems/number-of-visible-people-in-a-queue/)

This problem involves determining how many people each person in a queue can see if everyone in the queue is facing right. A person can see another person if the second person is taller than every person in between.

### Approaches
1. [Brute Force Approach](#brute-force-approach)
2. [Stack-Based Approach](#stack-based-approach)

### Brute Force Approach

The brute force approach involves a nested loop where for each person, we count how many people are visible by checking each subsequent person in the queue.
 
#### Intuition
For each person in the queue, move to the right and check the heights of the people. Once we find someone taller, the view is blocked for all people after that person.

#### Implementation
```python
def canSeePersonsCount(heights):
    n = len(heights)
    visible = [0] * n
    
    # For each person in the queue
    for i in range(n):
        max_height = 0
        # Move to the right in the queue to check how many the current person can see
        for j in range(i + 1, n):
            # If the height at j is greater than max_height seen so far from i
            if heights[j] > max_height:
                visible[i] += 1
                max_height = heights[j]
                # If we find a person taller than heights[i], stop as they can block vision
                if heights[j] > heights[i]:
                    break
    return visible
```

#### Time Complexity
- **Time:** O(n^2)  
- **Space:** O(n)  

### Stack-Based Approach

We can optimize the solution using a stack. This approach uses a stack to keep track of indices of people in the queue. 

#### Intuition
As we traverse the queue from right to left, we use a stack to maintain the indices of the visible people. If the current person is taller than or equal to the person indexed at the top of the stack, the current person can see that person, and potentially people beyond the one top of stack. 

#### Implementation
```python
def canSeePersonsCount(heights):
    n = len(heights)
    visible = [0] * n
    stack = []
    
    # Start from rightmost person
    for i in range(n - 1, -1, -1):
        # As long as the stack has people and current is taller or equal, they can be seen
        while stack and heights[i] > heights[stack[-1]]:
            stack.pop()
            visible[i] += 1
        
        # If there's still someone left in the stack, current person can also see them
        if stack:
            visible[i] += 1

        # Push current person's index to the stack
        stack.append(i)
        
    return visible
```

#### Time Complexity
- **Time:** O(n) 
- **Space:** O(n) because of the stack used to store indices.

In conclusion, the stack-based solution is optimal and efficiently reduces the time complexity to linear, by utilizing the data structure to consistently keep track of visible indices. Understanding these crucial observations can lead to significant code simplification and efficiency improvement in competitive programming scenarios.

