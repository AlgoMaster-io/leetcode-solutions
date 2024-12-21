# [Leetcode 895: Maximum Frequency Stack](https://leetcode.com/problems/maximum-frequency-stack/)

## Approaches

1. [Basic Approach: Using HashMap and Stack](#basic-approach-using-hashmap-and-stack)
2. [Optimized Approach: Frequency and Group Stack Map](#optimized-approach-frequency-and-group-stack-map)

---

### Basic Approach: Using HashMap and Stack

#### Intuition

The basic idea is to use two primary data structures: a hashmap to keep track of the frequency of each element, and a stack to store the elements as they come in. For the `push` operation, we increment the frequency of the element and push it onto the stack. For the `pop` operation, we must find the most frequently occurring element (the one with the maximum frequency). Once found, we should remove one occurrence of the element from the stack and decrement its frequency.

This solution works but is not optimal in terms of finding the most frequent element due to the linear search needed through the stack to correctly pop the element.

```cpp
#include <unordered_map>
#include <stack>
#include <vector>

class FreqStack {
    std::unordered_map<int, int> freq;           // To store the frequency of elements
    std::stack<int> elements;                    // To store elements

public:
    FreqStack() {}

    void push(int x) {
        // Increment the frequency of x
        freq[x]++;
        // Push the element onto the stack
        elements.push(x);
    }

    int pop() {
        // Vector to temporarily store elements popped from stack
        std::vector<int> temp;
        int maxFreq = 0;
        int topElement;
        
        // Find the element with the highest frequency
        while (!elements.empty()) {
            int curr = elements.top();
            elements.pop();
            if (freq[curr] > maxFreq) {
                maxFreq = freq[curr];
                topElement = curr;
            }
            temp.push_back(curr);
        }
        
        // Decrease the frequency of the popped element
        freq[topElement]--;
        
        // Push back elements onto the stack except the one with the highest frequency
        bool removed = false;
        for (int i = temp.size() - 1; i >= 0; --i) {
            if (temp[i] != topElement || removed) {
                elements.push(temp[i]);
            } else {
                removed = true;
            }
        }

        return topElement;
    }
};
```

- **Time Complexity**: 
  - `push`: O(1)
  - `pop`: O(n), where n is the total number of elements in the frequency stack, due to searching for the max frequency.
- **Space Complexity**: O(n), where n is the number of elements.

### Optimized Approach: Frequency and Group Stack Map

#### Intuition

In this optimized solution, we are aiming to optimize the `pop` operation to O(1) by making use of two hashmaps:
1. `freq` hashmap to store the frequency of each element.
2. `group` hashmap of stack to group elements by their frequency.

By maintaining these two structures, we can always access the group stack of the current maximum frequency directly and pop an element from there.

```cpp
#include <unordered_map>
#include <stack>

class FreqStack {
    std::unordered_map<int, int> freq;               // To store the frequency of elements
    std::unordered_map<int, std::stack<int>> group;  // To store stack of elements grouped by frequency
    int maxFreq = 0;                                 // To store the maximum frequency at any point
    
public:
    FreqStack() {}

    void push(int x) {
        // Increment the frequency of x
        freq[x]++;
        
        // Update maximum frequency
        if (freq[x] > maxFreq) {
            maxFreq = freq[x];
        }
        
        // Add x to the stack of its current frequency
        group[freq[x]].push(x);
    }

    int pop() {
        // Get element with maximum frequency
        int x = group[maxFreq].top();
        group[maxFreq].pop();
        
        // Decrease the frequency of the popped element
        freq[x]--;
        
        // If no elements are left on the max frequency stack, decrease max frequency
        if (group[maxFreq].empty()) {
            maxFreq--;
        }
        
        return x;
    }
};
```

- **Time Complexity**: 
  - `push`: O(1)
  - `pop`: O(1)
- **Space Complexity**: O(n), where n is the number of elements that have been inserted into the stack so far.

In this solution, both `push` and `pop` operations are optimized to run in constant time, offering significant performance improvements over the basic approach.

