# [Leetcode 895: Maximum Frequency Stack](https://leetcode.com/problems/maximum-frequency-stack/)

## Approaches:
- [Approach 1: Using a HashMap and Stack, Basic Approach](#approach-1)
- [Approach 2: Optimized Approach using Two HashMaps and a MaxFrequency variable](#approach-2)

---

### Approach 1: Using a HashMap and Stack, Basic Approach
**Intuition:**
The basic strategy is to use a single hash map to track the frequency of each element. Additionally, we use a stack to manage the elements. When popping, we need to ensure we remove the most recently added element with the highest frequency.

#### Steps:
1. **Hash Map to Track Frequencies:** Use a hash map (object) where the key is the element and the value is the frequency. This helps us determine how often each element appears.
2. **Stack for Element Management:** Maintain a stack that records the order of elements as they are pushed. While popping, this helps fetch the most recently added element.
3. **Pop Strategy:** Iterate over the stack from the top, find the element with the maximum frequency, and pop it. This makes popping operations costly as we scan through the stack.

Considering the inefficiencies when looking up elements to pop, especially since multiple elements with maximum frequency need revisiting, this approach is not optimal.

**Time Complexity:**  
- `push`: \(O(1)\) since updating a hash map and pushing to a stack take constant time.
- `pop`: \(O(n)\) where \(n\) is the number of elements in the stack due to needing to scan for the highest frequency element.

**Space Complexity:**  
- \(O(n)\) for the stack and frequencies hash map. 

Due to implementation complexity and inefficiencies, the code will not be provided for this approach.

---

### Approach 2: Optimized Approach using Two HashMaps and a MaxFrequency variable
**Intuition:**
A more optimal approach is to use two hash maps and a maxFrequency counter:
- One hash map keeps track of the frequency of each element.
- A second hash map maps each frequency to a stack of elements with that frequency. This enables quick access to the most recent element with a specific frequency.

#### Steps:
1. **Frequency HashMap (`freq`):** This maps elements to their current frequency count.
2. **Group HashMap (`group`):** This maps each frequency to a stack of elements that have the same frequency. These elements are stored as stacks to maintain the order of insertion while having constant time access.
3. **MaxFrequency Counter:** Track the maximum frequency at any time to quickly access the stack of elements with the highest frequency.

When popping, retrieve the topmost element from the stack that corresponds to the highest frequency. After removing, decrease the frequency of that element and adjust the `maxFrequency` if necessary.

**Time Complexity:**  
- `push`: \(O(1)\)
- `pop`: \(O(1)\)

**Space Complexity:**  
- \(O(n)\) for the frequency and group hash maps.

**TypeScript Code Implementation:**

```typescript
class FreqStack {
    private freq: Map<number, number>;
    private group: Map<number, number[]>;
    private maxFrequency: number;

    constructor() {
        this.freq = new Map();
        this.group = new Map();
        this.maxFrequency = 0;
    }

    push(val: number): void {
        // Update the element's frequency
        let frequency = this.freq.get(val) || 0;
        this.freq.set(val, frequency + 1);

        // Update the max frequency
        this.maxFrequency = Math.max(this.maxFrequency, frequency + 1);

        // Push this element onto the stack of its frequency level
        if (!this.group.has(frequency + 1)) {
            this.group.set(frequency + 1, []);
        }
        this.group.get(frequency + 1)?.push(val);
    }

    pop(): number {
        // Get the top element from the stack of the max frequency
        const stack = this.group.get(this.maxFrequency);
        const val = stack?.pop() || 0;

        // Decrement the frequency of that element
        this.freq.set(val, (this.freq.get(val) || 1) - 1);

        // If no elements are at the current max frequency, decrease max frequency
        if (stack?.length === 0) {
            this.maxFrequency--;
        }

        return val;
    }
}
```

This approach dramatically decreases the time spent finding the most frequent and recent element, offering a more efficient solution to the problem.

