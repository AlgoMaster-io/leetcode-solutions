# Random Pick with Weight
You are given an array of positive integers w where w[i] describes the weight of index i (0-indexed). You need to implement the function pickIndex(), which randomly picks an index in proportion to its weight.

- pickIndex() should return an index such that the probability of picking index i is w[i] / sum(w).
### Constraints:
- 1 <= w.length <= 10^4
- 1 <= w[i] <= 10^5
- pickIndex will be called at most 10^5 times.

### Examples
```javascript
Input:
["Solution","pickIndex"]
[[[1]],[]]

Output:
[null,0]

Explanation:
Solution solution = new Solution([1]);
solution.pickIndex(); // return 0
Since there is only one element, the function should always return 0.


Input:
["Solution","pickIndex","pickIndex","pickIndex","pickIndex","pickIndex"]
[[[1,3]],[],[],[],[],[]]

Output:
[null,1,1,1,1,0]

Explanation:
Solution solution = new Solution([1,3]);
solution.pickIndex(); // return 1 with probability 3/4, and return 0 with probability 1/4.
The picks are made randomly, so this output is just an example.
```

## Approaches to Solve the Problem
### Approach 1: Brute Force (Inefficient)
##### Intuition:
The simplest solution is to map each index i to a number of occurrences proportional to its weight w[i]. Then, randomly select an index from this expanded array. This is very inefficient in terms of both space and time, especially for large weights.

Steps:
1. Create a list where each index i appears w[i] times.
2. Randomly pick an index from this expanded list.
##### Time Complexity:
O(n * w_max), where n is the length of the input array and w_max is the maximum weight value. This is very inefficient for large arrays or large weights.
##### Space Complexity:
O(n * w_max), because we need to store the expanded list.
##### Python Code:
```python
import random

class Solution:
    def __init__(self, w):
        self.weights = []
        for i in range(len(w)):
            self.weights += [i] * w[i]
    
    def pickIndex(self):
        return random.choice(self.weights)
```
### Approach 2: Prefix Sum + Binary Search (Optimal Solution)
##### Intuition: 
A more efficient solution involves using the prefix sum of the weights to create a cumulative sum array. The idea is to transform the weights into a continuous probability distribution, where each weight occupies a segment proportional to its size.

- Construct a prefix sum array where each element at index i represents the cumulative weight up to that index.
- Use binary search to efficiently find which segment of the cumulative sum a randomly generated number falls into.

Steps:
1. Precompute the prefix sum of the weights.
   - Create a cumulative sum array where prefix_sum[i] = w[0] + w[1] + ... + w[i].
   - This transforms the weights into a continuous range.
2. Generate a random number r between 0 and total_sum (where total_sum is the sum of all weights).
3. Use binary search to find the first index i such that prefix_sum[i] >= r.
4. Return the index i.
##### Visualization:
```perl
For w = [1, 3], the prefix sum array will be [1, 4], meaning:

Index 0 has a range of [0, 1) (probability 1/4).
Index 1 has a range of [1, 4) (probability 3/4).
```
##### Time Complexity:
- O(n) for the precomputation step (building the prefix sum array).
- O(log n) for each call to pickIndex() because of the binary search.
##### Space Complexity:
- O(n), where n is the length of the input array, for storing the prefix sum array.
##### Python Code:
```python
import random
import bisect

class Solution:
    def __init__(self, w):
        self.prefix_sums = []
        prefix_sum = 0
        
        for weight in w:
            prefix_sum += weight
            self.prefix_sums.append(prefix_sum)
        
        self.total_sum = prefix_sum
    
    def pickIndex(self):
        # Generate a random number in the range [1, total_sum]
        target = random.randint(1, self.total_sum)
        
        # Binary search to find the index where the random number fits
        return bisect.bisect_left(self.prefix_sums, target)
```
### Explanation of Code:
- Precomputation (Constructor):
The constructor builds the prefix_sums array and calculates the total sum of weights. This preprocessing is done in O(n) time.

- pickIndex() Function:
In the pickIndex() function, a random integer between 1 and total_sum is generated, representing a point in the cumulative probability distribution. Then, bisect.bisect_left is used to perform binary search on the prefix_sums array to efficiently find the index corresponding to this point. This is done in O(log n) time.
### Edge Cases:
1. Single Weight: If the input array has only one element, pickIndex() should always return 0 because there's only one possible index.
2. Large Weights: The solution efficiently handles large weights using the prefix sum approach without expanding the list.
3. Small Weights: Even with small weights, the solution scales well because the binary search operates on the prefix sums rather than on the original weights.
## Summary
| Approach                         | Time Complexity | Space Complexity |
|-----------------------------------|-----------------|------------------|
| Brute Force                    | O(n * w_max)      | O(n * w_max)             |
| Prefix Sum + Binary Search (Optimal)	                          | 	O(n) for preprocessing, O(log n) for each pick            | O(n)             |

The Prefix Sum + Binary Search approach is the most efficient solution. It achieves O(n) time for preprocessing and O(log n) time for each call to pickIndex().