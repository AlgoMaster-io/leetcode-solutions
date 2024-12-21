# [Leetcode 895: Maximum Frequency Stack](https://leetcode.com/problems/maximum-frequency-stack/)

## Approaches
1. [Basic Approach: Brute-force with Stack](#basic-approach)
2. [Optimized Approach: Frequency and Grouped Stacks](#optimized-approach)

### Basic Approach: Brute-force with Stack

**Intuition:**

The problem requires us to mimic a stack but with the constraint of popping the most frequent element. If there is a tie in frequency, the most recently pushed element should be popped first. 

A basic approach is to maintain a simple stack and a frequency counter dictionary to keep track of the element count. On popping, we traverse the stack from top to bottom to find the element with the maximum frequency.

**Code Implementation:**

```java
import java.util.*;

class FreqStack {
    private Stack<Integer> stack;
    private Map<Integer, Integer> freqCount;

    public FreqStack() {
        stack = new Stack<>();
        freqCount = new HashMap<>();
    }
    
    public void push(int val) {
        stack.push(val);
        freqCount.put(val, freqCount.getOrDefault(val, 0) + 1);
    }
    
    public int pop() {
        int mostFreq = 0;
        int val = 0;
        
        // find the element with the maximum frequency
        for (int i = stack.size() - 1; i >= 0; i--) {
            int element = stack.get(i);
            int count = freqCount.get(element);
            if (count > mostFreq) {
                mostFreq = count;
                val = element;
            }
        }
        
        // update the stack and frequency count
        stack.remove(stack.lastIndexOf(val));
        freqCount.put(val, mostFreq - 1);
        
        return val;
    }
}
```

**Time Complexity:**
- `push(val)`: O(1) each time we push an element.
- `pop()`: O(n) since we have to traverse the whole stack to find the element with the maximum frequency.

**Space Complexity:** 
- O(n) where n is the number of elements in the stack.

### Optimized Approach: Frequency and Grouped Stacks

**Intuition:**

To optimize, we can use two hash maps:
- `frequency` to keep track of the number of occurrences of each element.
- `group` to organize elements by their frequency, where each index in `group` represents a stack of elements with that frequency.

This ensures that when we are popping, we can instantly retrieve the most frequent and recent element by looking at the top element of the stack at the current maximum frequency.

**Code Implementation:**

```java
import java.util.*;

class FreqStack {
    private Map<Integer, Integer> frequencyMap;
    private Map<Integer, Stack<Integer>> groupStackMap;
    private int maxFrequency;

    public FreqStack() {
        frequencyMap = new HashMap<>();
        groupStackMap = new HashMap<>();
        maxFrequency = 0;
    }
    
    public void push(int val) {
        int freq = frequencyMap.getOrDefault(val, 0) + 1;
        frequencyMap.put(val, freq);
        
        if (freq > maxFrequency) {
            maxFrequency = freq;
        }
        
        groupStackMap.computeIfAbsent(freq, z -> new Stack<>()).push(val);
    }
    
    public int pop() {
        Stack<Integer> stack = groupStackMap.get(maxFrequency);
        int val = stack.pop();
        
        frequencyMap.put(val, frequencyMap.get(val) - 1);
        
        if (stack.isEmpty()) {
            maxFrequency--;
        }
        
        return val;
    }
}
```

**Time Complexity:**
- `push(val)`: O(1) since we just increment counters and update stacks.
- `pop()`: O(1) for directly accessing and popping from the stack of the maximum frequency.

**Space Complexity:**
- O(n) where n is the number of elements in the stack.

This optimized solution uses the power of hash mappings to provide an efficient way to manage frequency and direct access to elements by grouping them in stacks based on their frequency.

