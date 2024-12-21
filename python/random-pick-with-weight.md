# [Leetcode 528: Random Pick with Weight](https://leetcode.com/problems/random-pick-with-weight/)

## Approaches:
1. [Prefix Sum and Binary Search](#prefix-sum-and-binary-search)
2. [Cumulative Sum with Random Choice](#cumulative-sum-with-random-choice)

---

### Approach 1: Prefix Sum and Binary Search

**Intuition:**
The problem requires picking indices based on given weights such that the probability of picking an index is proportional to its weight. This can be effectively handled using a prefix sum array and binary search.
- We will first compute a prefix sum array, where each element at index `i` is the sum of weights from index `0` to `i`.
- The prefix sum array is essential as it allows us to determine the probability range for each index.

**Steps:**
1. Compute a prefix sum of the weights array.
2. To simulate the random pick, generate a random integer between `1` and the last element of the prefix sum array.
3. Use binary search to find the smallest index whose prefix sum is at least the random number. This index corresponds to the weighted random pick.

**Code:**

```python
import random
import bisect

class Solution:
    def __init__(self, w: List[int]):
        self.prefix_sum = []
        current_sum = 0
        for weight in w:
            current_sum += weight
            self.prefix_sum.append(current_sum)
        
    def pickIndex(self) -> int:
        # Generate a random number between 1 and the total sum of weights
        target = random.randint(1, self.prefix_sum[-1])
        # Use binary search to find the right index
        index = bisect.bisect_left(self.prefix_sum, target)
        return index
```

**Time Complexity:** 
- Initialization: O(n), where `n` is the number of weights.
- Picking: O(log n) due to the binary search.

**Space Complexity:**
- O(n) to store the prefix sum array.

---

### Approach 2: Cumulative Sum with Random Choice

**Intuition:**
Another approach to solve this problem is using cumulative probability distribution. We can modify the weights into a cumulative distribution and utilize random choice based on this distribution.

**Steps:**
1. Normalize the weights to a cumulative distribution.
2. Use the `random.choices` method which allows for weighted random selection based on normalized probabilities.

**Code:**

```python
import random
import itertools

class Solution:
    def __init__(self, w: List[int]):
        self.total_sum = sum(w)
        self.normalized_weights = [i / self.total_sum for i in itertools.accumulate(w)]

    def pickIndex(self) -> int:
        target = random.random()
        # Use linear search to find which cumulative weight the target fits in
        for i, weight in enumerate(self.normalized_weights):
            if target < weight:
                return i
```

**Time Complexity:** 
- Initialization: O(n) to compute cumulative sum and normalization.
- Picking: O(n) since we might need to iterate over all weights in the worst case.

**Space Complexity:**
- O(n) to store the normalized cumulative weights.

---

These approaches provide a structured way to handle the "pick index with weight" problem using various probability distribution techniques. Each method has its own potential use case based on the constraints and requirements of execution speed and memory.

