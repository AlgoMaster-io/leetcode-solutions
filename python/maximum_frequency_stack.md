# 895. [Maximum Frequency Stack](https://leetcode.com/problems/maximum-frequency-stack/)

## Approach: HashMap with Stack for Frequency Tracking

### Solution
```python
# Time Complexity:
#   - push: O(1)
#   - pop: O(1)
# Space Complexity: O(n), where n is the number of elements in the stack

from collections import defaultdict
from collections import deque

class FreqStack:
    def __init__(self):
        self.freq_map = defaultdict(int)  # Maps element to its frequency
        self.group_map = defaultdict(deque)  # Maps frequency to a deque of elements
        self.max_freq = 0  # Tracks the maximum frequency

    def push(self, val: int) -> None:
        # Update frequency of the element
        freq = self.freq_map[val] + 1
        self.freq_map[val] = freq

        # Update max frequency
        self.max_freq = max(self.max_freq, freq)

        # Add the element to the deque corresponding to its frequency
        self.group_map[freq].append(val)

    def pop(self) -> int:
        # Pop the most frequent element
        val = self.group_map[self.max_freq].pop()

        # Decrement the frequency of the popped element
        self.freq_map[val] -= 1

        # If no elements are left at the current max frequency, decrement max_freq
        if not self.group_map[self.max_freq]:
            self.max_freq -= 1

        return val

# Your FreqStack object will be instantiated and called as such:
# obj = FreqStack()
# obj.push(val)
# param_2 = obj.pop()
```

