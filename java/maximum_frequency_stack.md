# 895. [Maximum Frequency Stack](https://leetcode.com/problems/maximum-frequency-stack/)

## Approach: HashMap with Stack for Frequency Tracking

### Solution
```java
// Time Complexity:
//   - push: O(1)
//   - pop: O(1)
// Space Complexity: O(n), where n is the number of elements in the stack
import java.util.HashMap;
import java.util.Stack;

class FreqStack {
    private HashMap<Integer, Integer> freqMap; // Maps element to its frequency
    private HashMap<Integer, Stack<Integer>> groupMap; // Maps frequency to a stack of elements
    private int maxFreq; // Tracks the maximum frequency

    public FreqStack() {
        freqMap = new HashMap<>();
        groupMap = new HashMap<>();
        maxFreq = 0;
    }

    public void push(int val) {
        // Update frequency of the element
        int freq = freqMap.getOrDefault(val, 0) + 1;
        freqMap.put(val, freq);

        // Update max frequency
        maxFreq = Math.max(maxFreq, freq);

        // Add the element to the stack corresponding to its frequency
        groupMap.putIfAbsent(freq, new Stack<>());
        groupMap.get(freq).push(val);
    }

    public int pop() {
        // Pop the most frequent element
        int val = groupMap.get(maxFreq).pop();

        // Decrement the frequency of the popped element
        freqMap.put(val, freqMap.get(val) - 1);

        // If no elements are left at the current max frequency, decrement maxFreq
        if (groupMap.get(maxFreq).isEmpty()) {
            maxFreq--;
        }

        return val;
    }
}

/**
 * Your FreqStack object will be instantiated and called as such:
 * FreqStack obj = new FreqStack();
 * obj.push(val);
 * int param_2 = obj.pop();
 */
```