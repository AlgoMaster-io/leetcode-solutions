# 895. [Maximum Frequency Stack](https://leetcode.com/problems/maximum-frequency-stack/)

## Approach: HashMap with Stack for Frequency Tracking

### Solution
csharp
```csharp
// Time Complexity:
//   - push: O(1)
//   - pop: O(1)
// Space Complexity: O(n), where n is the number of elements in the stack
using System.Collections.Generic;

public class FreqStack {
    private Dictionary<int, int> freqMap; // Maps element to its frequency
    private Dictionary<int, Stack<int>> groupMap; // Maps frequency to a stack of elements
    private int maxFreq; // Tracks the maximum frequency

    public FreqStack() {
        freqMap = new Dictionary<int, int>();
        groupMap = new Dictionary<int, Stack<int>>();
        maxFreq = 0;
    }

    public void Push(int val) {
        // Update frequency of the element
        if (!freqMap.ContainsKey(val)) {
            freqMap[val] = 0;
        }
        int freq = freqMap[val] + 1;
        freqMap[val] = freq;

        // Update max frequency
        maxFreq = Math.Max(maxFreq, freq);

        // Add the element to the stack corresponding to its frequency
        if (!groupMap.ContainsKey(freq)) {
            groupMap[freq] = new Stack<int>();
        }
        groupMap[freq].Push(val);
    }

    public int Pop() {
        // Pop the most frequent element
        int val = groupMap[maxFreq].Pop();

        // Decrement the frequency of the popped element
        freqMap[val]--;

        // If no elements are left at the current max frequency, decrement maxFreq
        if (groupMap[maxFreq].Count == 0) {
            maxFreq--;
        }

        return val;
    }
}

/**
 * Your FreqStack object will be instantiated and called as such:
 * FreqStack obj = new FreqStack();
 * obj.Push(val);
 * int param_2 = obj.Pop();
 */
```

