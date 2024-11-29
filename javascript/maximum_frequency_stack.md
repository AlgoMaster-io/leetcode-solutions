# 895. [Maximum Frequency Stack](https://leetcode.com/problems/maximum-frequency-stack/)

## Approach: HashMap with Stack for Frequency Tracking

### Solution
```javascript
// Time Complexity:
//   - push: O(1)
//   - pop: O(1)
// Space Complexity: O(n), where n is the number of elements in the stack

class FreqStack {
    constructor() {
        this.freqMap = new Map(); // Maps element to its frequency
        this.groupMap = new Map(); // Maps frequency to a stack of elements
        this.maxFreq = 0; // Tracks the maximum frequency
    }

    push(val) {
        // Update frequency of the element
        let freq = (this.freqMap.get(val) || 0) + 1;
        this.freqMap.set(val, freq);

        // Update max frequency
        this.maxFreq = Math.max(this.maxFreq, freq);

        // Add the element to the stack corresponding to its frequency
        if (!this.groupMap.has(freq)) {
            this.groupMap.set(freq, []);
        }
        this.groupMap.get(freq).push(val);
    }

    pop() {
        // Pop the most frequent element
        let val = this.groupMap.get(this.maxFreq).pop();

        // Decrement the frequency of the popped element
        this.freqMap.set(val, this.freqMap.get(val) - 1);

        // If no elements are left at the current max frequency, decrement maxFreq
        if (this.groupMap.get(this.maxFreq).length === 0) {
            this.maxFreq--;
        }

        return val;
    }
}

/**
 * Your FreqStack object will be instantiated and called as such:
 * let obj = new FreqStack();
 * obj.push(val);
 * let param_2 = obj.pop();
 */
```

