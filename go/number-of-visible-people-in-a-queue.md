# [Leetcode 1944: Number of Visible People in a Queue](https://leetcode.com/problems/number-of-visible-people-in-a-queue/)

## Approaches:
1. [Brute Force Solution](#brute-force-solution)
2. [Optimized Stack Solution](#optimized-stack-solution)

---

### Brute Force Solution

#### Intuition:
The basic idea of the brute force approach is to iterate through each person in the queue and check how many people they can see. A person can see someone if no one taller stands in between them.

#### Approach:
1. For each person in the queue, loop through the people standing on their right.
2. Maintain a count of visible persons.
3. Stop counting if a taller person is encountered, as they block the view further.

#### Code:
```go
func canSeePersonsCount(heights []int) []int {
    n := len(heights)
    result := make([]int, n)

    // Iterate over each person
    for i := 0; i < n; i++ {
        // Keep track of visible persons for each person
        count := 0
        // Iterate over persons to the right of current person
        for j := i + 1; j < n; j++ {
            // If the person is visible, increase count
            if heights[j] > heights[i] {
                count++
                break
            } else {
                count++
            }
        }
        result[i] = count
    }

    return result
}
```

#### Time Complexity:
- **O(n^2)**: For each person, we loop over all persons to their right.

#### Space Complexity:
- **O(1)**: Additional space is used for the result which is proportional to output size.

---

### Optimized Stack Solution

#### Intuition:
The optimized approach uses a stack to keep track of indices of people who can potentially block the view. We iterate from right to left, popping from the stack until we find a person taller than the current one. This allows us to efficiently determine how many people are visible for each person in constant amortized time.

#### Approach:
1. Initialize a stack to keep track of indices.
2. Iterate from the last person to the first.
3. While there are people in the stack shorter than the current person, they are visible, so pop them.
4. Record the count of pop operations for each person, then push the current person onto the stack.
5. The remaining top of the stack (if any) blocks any further view, so only counts till then.

#### Code:
```go
func canSeePersonsCount(heights []int) []int {
    n := len(heights)
    result := make([]int, n)
    stack := []int{}

    // Iterate over persons from right to left
    for i := n - 1; i >= 0; i-- {
        // While stack is not empty and the current person is taller or equal to 
        // the top person in the stack, they can see over them
        for len(stack) > 0 && heights[i] > heights[stack[len(stack)-1]] {
            stack = stack[:len(stack)-1]
            result[i]++
        }
        
        // If the stack is not empty, the current person can at least see 
        // the one on top of the stack before being blocked
        if len(stack) > 0 {
            result[i]++
        }
        
        // Push current person's index onto the stack
        stack = append(stack, i)
    }

    return result
}
```

#### Time Complexity:
- **O(n)**: Each person is pushed and popped from the stack at most once.

#### Space Complexity:
- **O(n)**: For the stack, in the worst case scenario.

---

