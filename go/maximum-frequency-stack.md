# [Leetcode 895: Maximum Frequency Stack](https://leetcode.com/problems/maximum-frequency-stack/)

## Approaches
- [Approach 1: Using Hashmaps and Stack](#approach-1-using-hashmaps-and-stack)

---

## Approach 1: Using Hashmaps and Stack

### Intuition:
The challenge of this problem is to efficiently manage the frequency of the elements and retrieve the most frequent one. We need a structure that can push elements, count their frequency, and retrieve the one with the highest frequency when popping. Moreover, in tie situations, the most recent element must be returned. 

The basic idea is to use two hashmaps:
1. `freq`: to keep track of the frequency of each element.
2. `group`: to map each frequency to a stack of numbers with that frequency.

Additionally, we maintain a `maxFreq` to store the current highest frequency. When we push an element, we increase its frequency in `freq` and then push it onto the appropriate stack in `group`. When popping, we take the element from the stack in `group` corresponding to `maxFreq`.

### Steps:
1. On `push(x)`, increase the frequency count of `x` in the `freq` map. 
2. Check if this new frequency surpasses `maxFreq`. If so, update `maxFreq`.
3. Push `x` onto the stack in `group` associated with its frequency.
4. On `pop()`, take the most recently added element from the stack corresponding to `maxFreq`.
5. Decrease its frequency in `freq`. If the stack for `maxFreq` is empty post-pop, decrement `maxFreq`.

### Code:
```go
type FreqStack struct {
    freq  map[int]int        // Maps values to their frequency
    group map[int][]int      // Maps frequency to stack of elements
    maxFreq int              // Keeps track of the current maximum frequency
}

func Constructor() FreqStack {
    return FreqStack{
        freq:   make(map[int]int),
        group:  make(map[int][]int),
        maxFreq: 0,
    }
}

func (this *FreqStack) Push(x int) {
    // Increase the frequency of the element x
    this.freq[x]++
    f := this.freq[x]

    // Update the max frequency if necessary
    if f > this.maxFreq {
        this.maxFreq = f
    }

    // Push x onto the stack at frequency f in the group
    this.group[f] = append(this.group[f], x)
}

func (this *FreqStack) Pop() int {
    // Get the most frequent element that was pushed last
    topOfStack := this.group[this.maxFreq]
    size := len(topOfStack)
    x := topOfStack[size-1]

    // Remove that element from the stack
    this.group[this.maxFreq] = topOfStack[:size-1]

    // Decrease the frequency of the popped element
    this.freq[x]--

    // If no more elements have this max frequency, decrease maxFreq
    if len(this.group[this.maxFreq]) == 0 {
        this.maxFreq--
    }

    return x
}
```

### Complexity Analysis:
- **Time Complexity**: Each `push` and `pop` operation runs in O(1) time because all operations (increasing frequency, updating max frequency, and pushing/popping from a map-backed stack) can be done in constant time.
- **Space Complexity**: O(n), where n is the number of distinct elements pushed onto the stack. This accounts for storing frequency counts in the `freq` hashmap and storing stacks of elements in `group`.

This optimal approach ensures efficient management of the stack operations while maintaining the requirements of retrieving the most frequent, recent value upon popping.

