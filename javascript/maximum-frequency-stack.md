[Leetcode Problem 895: Maximum Frequency Stack](https://leetcode.com/problems/maximum-frequency-stack/)

### Approaches:
- [Approach 1: Naive Stack and Frequency Count](#approach-1-naive-stack-and-frequency-count)
- [Approach 2: Optimized Frequency Stack Using Multiple Stacks](#approach-2-optimized-frequency-stack-using-multiple-stacks)

---

### Approach 1: Naive Stack and Frequency Count

**Intuition:**

In this naive approach, we use a stack to store the elements as they are pushed. Additionally, we keep a frequency map to track how many times each element appears. When we need to pop, we iterate through the stack from top to bottom to find the most frequent element, and remove it.

**Detailed Steps:**

1. Use a stack to store the elements as they are pushed.
2. Use a frequency map to track the count of each element.
3. To pop an element, iterate over the stack to find the element with the highest frequency, remove it, and then update the frequency map accordingly.

**Code:**

```javascript
class FreqStack {
    constructor() {
        this.stack = [];
        this.frequencyMap = {};
    }
    
    push(val) {
        // Increment frequency count for the value
        if (!this.frequencyMap[val]) {
            this.frequencyMap[val] = 0;
        }
        this.frequencyMap[val]++;
        
        // Push the value onto the stack
        this.stack.push(val);
    }
    
    pop() {
        let mostFreq = 0;
        let mostFreqVal;
        
        // Find the most frequently occurring element
        for (let val of this.stack) {
            if (this.frequencyMap[val] > mostFreq) {
                mostFreq = this.frequencyMap[val];
                mostFreqVal = val;
            }
        }
        
        // Find the last occurrence and remove it
        for (let i = this.stack.length - 1; i >= 0; i--) {
            if (this.stack[i] === mostFreqVal) {
                this.stack.splice(i, 1);
                break;
            }
        }
        
        // Decreasing the frequency of the most frequent element
        this.frequencyMap[mostFreqVal]--;
        if (this.frequencyMap[mostFreqVal] === 0) {
            delete this.frequencyMap[mostFreqVal];
        }
        
        return mostFreqVal;
    }
}
```

**Time Complexity:**
- `push`: O(1)
- `pop`: O(n), where n is the number of elements in the stack.

**Space Complexity:**
- O(n) due to storing all elements in the stack and frequency map.

---

### Approach 2: Optimized Frequency Stack Using Multiple Stacks

**Intuition:**

The most efficient way is to maintain several stacks, one for each frequency level. We also maintain a map that associates each element with its frequency. Additionally, we track the maximum frequency at any time. When popping, we pop from the stack corresponding to the current maximum frequency, which gives O(1) complexity.

**Detailed Steps:**

1. Use a map `frequencyMap` to keep track of the frequency of each element.
2. Use another map `groupStacks` where each frequency level stores a stack of elements.
3. Track the highest frequency with a variable `maxFreq`.
4. During a push operation, increment the frequency of the element and push it into the corresponding frequency stack. Update `maxFreq` if necessary.
5. During a pop operation, pop from the stack corresponding to `maxFreq` and decrement its frequency. Adjust `maxFreq` if the stack is empty after popping.

**Code:**

```javascript
class FreqStack {
    constructor() {
        this.frequencyMap = {};
        this.groupStacks = {};
        this.maxFreq = 0;
    }
    
    push(val) {
        // Increment frequency of the value
        if (!this.frequencyMap[val]) {
            this.frequencyMap[val] = 0;
        }
        this.frequencyMap[val]++;
        
        // Get the current frequency
        const freq = this.frequencyMap[val];
        
        // Update max frequency if needed
        if (freq > this.maxFreq) {
            this.maxFreq = freq;
        }
        
        // Push the value to the appropriate frequency stack
        if (!this.groupStacks[freq]) {
            this.groupStacks[freq] = [];
        }
        this.groupStacks[freq].push(val);
    }
    
    pop() {
        // Remove the element from the stack holding values at current max frequency
        const val = this.groupStacks[this.maxFreq].pop();
        
        // Decrease the frequency
        this.frequencyMap[val]--;
        if (this.frequencyMap[val] === 0) {
            delete this.frequencyMap[val];
        }
        
        // If no more elements at current max frequency, decrease max frequency
        if (this.groupStacks[this.maxFreq].length === 0) {
            delete this.groupStacks[this.maxFreq];
            this.maxFreq--;
        }
        
        return val;
    }
}
```

**Time Complexity:**
- `push`: O(1)
- `pop`: O(1)

**Space Complexity:**
- O(n) for maintaining the stacks and frequency maps. 

This optimal approach ensures both push and pop operations are efficient by utilizing the frequency map and multiple stacks to handle element retrievals at the maximum frequency in constant time.

