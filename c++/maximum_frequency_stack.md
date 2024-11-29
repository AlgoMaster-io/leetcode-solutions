# 895. [Maximum Frequency Stack](https://leetcode.com/problems/maximum-frequency-stack/)

## Approach: HashMap with Stack for Frequency Tracking

### Solution
```cpp
// Time Complexity:
//   - push: O(1)
//   - pop: O(1)
// Space Complexity: O(n), where n is the number of elements in the stack
#include <unordered_map>
#include <stack>

class FreqStack {
private:
    std::unordered_map<int, int> freqMap; // Maps element to its frequency
    std::unordered_map<int, std::stack<int>> groupMap; // Maps frequency to a stack of elements
    int maxFreq; // Tracks the maximum frequency

public:
    FreqStack() {
        maxFreq = 0;
    }

    void push(int val) {
        // Update frequency of the element
        int freq = freqMap[val] + 1;
        freqMap[val] = freq;

        // Update max frequency
        maxFreq = std::max(maxFreq, freq);

        // Add the element to the stack corresponding to its frequency
        groupMap[freq].push(val);
    }

    int pop() {
        // Pop the most frequent element
        int val = groupMap[maxFreq].top();
        groupMap[maxFreq].pop();

        // Decrement the frequency of the popped element
        freqMap[val]--;

        // If no elements are left at the current max frequency, decrement maxFreq
        if (groupMap[maxFreq].empty()) {
            maxFreq--;
        }

        return val;
    }
};

/**
 * Your FreqStack object will be instantiated and called as such:
 * FreqStack* obj = new FreqStack();
 * obj->push(val);
 * int param_2 = obj->pop();
 */
```

