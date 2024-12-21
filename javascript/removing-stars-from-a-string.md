# [LeetCode 2390: Removing Stars From a String](https://leetcode.com/problems/removing-stars-from-a-string/)

## Approaches
- [Approach 1: Stack Simulation](#approach-1-stack-simulation)
- [Approach 2: In-Place Simulation](#approach-2-in-place-simulation)

---

## Approach 1: Stack Simulation

### Intuition
The problem involves removing characters from the string when a star '*' is encountered, where the removal operation is applied to the last character before the star. This behavior can be efficiently mimicked using a stack data structure. By iterating through the string, we'll push characters onto the stack when they're not stars, and pop the stack when we encounter a star. The nature of stacks (Last In First Out) enables us to reverse the removal operation required by the problem at hand.

### Code
```javascript
function removeStars(s) {
    let stack = [];
    
    for (let char of s) {
        if (char === '*') {
            // We pop the last character, essentially "removing" it 
            stack.pop();
        } else {
            // Push the current character onto the stack
            stack.push(char);
        }
    }
    
    // Join stack into result string
    return stack.join('');
}
```

### Time Complexity
- **O(n)**: We iterate through the string once, with each operation on the stack (push/pop) being O(1).

### Space Complexity
- **O(n)**: In the worst case, all characters are retained (no stars) requiring storage for all characters.

---

## Approach 2: In-Place Simulation

### Intuition
Instead of utilizing an explicit stack data structure, we can treat the input string itself as semi-stack using a pointer to track the indices. This is done by overwriting characters directly in the string. Every time a non-star character is encountered, it is placed at the current position of the pointer, which simulates the stack's push operation. When a star is found, it signifies a removal action that simply decrements the pointer, akin to a pop operation.

### Code
```javascript
function removeStars(s) {
    let resultArr = Array(s.length);
    let point = 0;
    
    for (let char of s) {
        if (char === '*') {
            // Move pointer back, simulating a pop in the stack
            point--;
        } else {
            // Place current character in "stack" simulation
            resultArr[point] = char;
            point++;
        }
    }
    
    // Build final string from characters in the "stack"
    return resultArr.slice(0, point).join('');
}
```

### Time Complexity
- **O(n)**: We traverse the string a single time. The in-place operations (overwriting, pointer navigation) are each O(1).

### Space Complexity
- **O(n)**: An auxiliary array is used, but this is theoretically less than a full stack from the initial approach, as we do not store unnecessary star characters.

--- 

These methods offer viable solutions to the problem, utilizing either a stack structure or in-place array manipulation, providing varying space complexities while maintaining linear time execution.

