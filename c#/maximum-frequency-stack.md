# [Leetcode 895: Maximum Frequency Stack](https://leetcode.com/problems/maximum-frequency-stack/)

## Approaches
- [Approach 1: Basic Stack with Frequency Count](#approach-1-basic-stack-with-frequency-count)
- [Approach 2: Optimized Stack with Max Frequency](#approach-2-optimized-stack-with-max-frequency)

## Approach 1: Basic Stack with Frequency Count

### Intuition:
The idea is to maintain a regular stack along with a frequency map of all elements pushed onto the stack. When popping, we need to find the element with the highest frequency. This can be done by scanning the frequency map each time which is not efficient but serves as a good starting point.

### Implementation:

```csharp
public class FreqStack {
    private Dictionary<int, int> frequency;
    private List<int> stack;

    public FreqStack() {
        frequency = new Dictionary<int, int>();
        stack = new List<int>();
    }
    
    public void Push(int x) {
        // Add element to the stack
        stack.Add(x);
        
        // Update the frequency map
        if (!frequency.ContainsKey(x))
            frequency[x] = 0;
        frequency[x]++;
    }
    
    public int Pop() {
        // Find the element with the maximum frequency
        int maxFreq = 0;
        foreach (var kvp in frequency) {
            if (kvp.Value > maxFreq) {
                maxFreq = kvp.Value;
            }
        }
        
        // Scan the stack backwards to find the highest frequency element
        for (int i = stack.Count - 1; i >= 0; i--) {
            if (frequency[stack[i]] == maxFreq) {
                int result = stack[i];

                // Decrease its frequency count
                frequency[result]--;
                if (frequency[result] == 0) 
                    frequency.Remove(result);

                // Remove from the stack
                stack.RemoveAt(i);
                return result;
            }
        }

        return -1; // Should not happen if methods are called correctly.
    }
}
```

### Complexity Analysis:
- **Time Complexity**: 
  - `Push` operation is O(1).
  - `Pop` operation is O(n) due to scanning of the stack and frequency dictionary.
- **Space Complexity**: O(n), where n is the number of elements pushed onto the stack.

## Approach 2: Optimized Stack with Max Frequency

### Intuition:
Instead of pairing a regular stack with a frequency dictionary and scanning every time, we can track the maximum frequency and build a list of stacks corresponding to each frequency. This allows constant time operation for both pushing and popping the most frequent elements.

### Implementation:

```csharp
public class FreqStack {
    private Dictionary<int, int> frequency;
    private Dictionary<int, Stack<int>> freqStacks;
    private int maxFreq;

    public FreqStack() {
        frequency = new Dictionary<int, int>();
        freqStacks = new Dictionary<int, Stack<int>>();
        maxFreq = 0;
    }
    
    public void Push(int x) {
        // Update frequency count
        if (!frequency.ContainsKey(x))
            frequency[x] = 0;
        frequency[x]++;

        // Update max frequency
        int freq = frequency[x];
        if (freq > maxFreq)
            maxFreq = freq;

        // Add to the stack corresponding to this frequency
        if (!freqStacks.ContainsKey(freq))
            freqStacks[freq] = new Stack<int>();
        freqStacks[freq].Push(x);
    }
    
    public int Pop() {
        // Get the most frequent element
        int result = freqStacks[maxFreq].Pop();

        // Decrease the frequency count
        frequency[result]--;
        if (freqStacks[maxFreq].Count == 0) {
            freqStacks.Remove(maxFreq);
            maxFreq--; // Decrease maxFreq when no elements left at this level
        }
        
        return result;
    }
}
```

### Complexity Analysis:
- **Time Complexity**: O(1) for both push and pop operations.
- **Space Complexity**: O(n), where n is the number of elements pushed onto the stack.
  
By refining the above solution, the stack operations become much more efficient especially for large input sizes, reducing the time complexity which is critical for performance in competitive programming.

