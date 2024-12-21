## [Leetcode 895: Maximum Frequency Stack](https://leetcode.com/problems/maximum-frequency-stack/)

### Approaches

- [Approach 1: Use a List and Dictionary](#approach-1-use-a-list-and-dictionary)
- [Approach 2: Optimized Using Two Dictionaries](#approach-2-optimized-using-two-dictionaries)

---

### Approach 1: Use a List and Dictionary

**Intuition:**

We need a data structure that allows us to push elements with their frequency and also pop the most frequent element. We can leverage a stack (List in Python) to store elements and a Dictionary to keep track of the frequency of each element. 

**Detailed Steps:**

1. Maintain a `List` (acting as a stack) for keeping the elements in order.
2. Use a `Dictionary` to store the frequency count of each element.
3. For `push(x)`, add the element `x` to the stack and update its frequency in the dictionary.
4. For `pop()`, find the most frequent element. In case of a tie, return the element closest to the top of the stack.
5. Iterate from the end of the stack to find the most frequent element and reduce its frequency.
6. Remove that element from the stack.

**Code:**

```python
class FreqStack:
    def __init__(self):
        self.stack = []  # To store the elements
        self.freq = {}   # To store the frequency of each element

    def push(self, x: int) -> None:
        # Push the element onto the stack
        self.stack.append(x)
        # Update the frequency of the element
        if x in self.freq:
            self.freq[x] += 1
        else:
            self.freq[x] = 1

    def pop(self) -> int:
        # Find the maximum frequency among the elements in the stack
        max_frequency = max(self.freq.values())
        # Iterate over the stack from the end to find the most frequent element
        for i in range(len(self.stack) - 1, -1, -1):
            if self.freq[self.stack[i]] == max_frequency:
                popped_element = self.stack.pop(i)
                # Decrease the frequency of the element
                self.freq[popped_element] -= 1
                if self.freq[popped_element] == 0:
                    del self.freq[popped_element]
                return popped_element
        
```

**Complexity Analysis:**

- **Time Complexity:** 
  - `push(x)` takes O(1) because we just append and update the dictionary.
  - `pop()` takes O(n) in the worst case as we might traverse the entire list to find the element with the maximum frequency.
  
- **Space Complexity:** O(n), where n is the number of elements because we store all elements in the stack and their frequency count in a dictionary.

---

### Approach 2: Optimized Using Two Dictionaries

**Intuition:**

To make `pop()` efficient, we can use a second `Dictionary` to map frequencies to stacks of elements. This allows us to directly access the stack with the current maximum frequency. We'll also maintain a variable to track the current maximum frequency.

**Detailed Steps:**

1. Maintain a `Dictionary` (`freq`) that maps an element to its frequency count.
2. Maintain another `Dictionary` (`group`) that maps a frequency count to a stack of elements with that frequency.
3. Maintain a `max_freq` variable to track the current maximum frequency.
4. For `push(x)`, increment the frequency of `x`, update `group[freq[x]]`, and adjust `max_freq` if necessary.
5. For `pop()`, pop the element from `group[max_freq]`, update its frequency, and update `max_freq` if the stack for the current max frequency is empty.

**Code:**

```python
class FreqStack:

    def __init__(self):
        self.freq = {}  # Frequency of each element
        self.group = {}  # Elements grouped by their frequencies
        self.max_freq = 0  # Maximum frequency

    def push(self, x: int) -> None:
        # Update the frequency of x
        self.freq[x] = 1 + self.freq.get(x, 0)
        # Update max frequency if necessary
        max_freq = self.freq[x]
        if max_freq > self.max_freq:
            self.max_freq = max_freq
        # Push x onto the group stack corresponding to its frequency
        if max_freq not in self.group:
            self.group[max_freq] = []
        self.group[max_freq].append(x)

    def pop(self) -> int:
        # Get and remove the top element from the stack of the max frequency
        x = self.group[self.max_freq].pop()
        # Decrease the frequency of the element
        self.freq[x] -= 1
        # If no element has the current max frequency, decrease the max frequency
        if not self.group[self.max_freq]:
            self.max_freq -= 1
        return x
```

**Complexity Analysis:**

- **Time Complexity:** 
  - `push(x)` takes O(1) due to dictionary updates and appends.
  - `pop()` takes O(1) because we access and modify the top of the stack for the current maximum frequency.
  
- **Space Complexity:** O(n), where n is the number of elements, due to storage of the elements and grouped frequencies.

By using this approach, we optimize the `pop()` to run in constant time by keeping track of elements based on their frequency direct access to the current maximum frequency group.

