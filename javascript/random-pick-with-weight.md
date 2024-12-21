# [Leetcode 528: Random Pick with Weight](https://leetcode.com/problems/random-pick-with-weight/)

## Approaches
1. [Prefix Sum and Linear Search](#prefix-sum-and-linear-search)
2. [Prefix Sum and Binary Search](#prefix-sum-and-binary-search)

### Approach 1: Prefix Sum and Linear Search

#### Intuition
The problem requires us to pick an index randomly, but with a weight proportional to the values in the `w` array. A simple approach would be to create a prefix sum array, where each entry corresponds to the cumulative weight. We then generate a random number `x` between 0 and the total sum of weights. The selected index is the first index in the prefix sum array that is greater than or equal to `x`.

#### Steps
1. Compute the prefix sum array. Each element in this array is the sum of all previous weights.
2. Generate a random integer from 0 to the total sum of weights minus 1.
3. Use a linear search to find the first index in the prefix sum array that is greater than or equal to this random integer.
4. Return the found index.

```javascript
class Solution {
    constructor(w) {
        this.prefixSums = [];
        let sum = 0;
        
        // Create the prefix sum array
        for (let weight of w) {
            sum += weight;
            this.prefixSums.push(sum);
        }
        
        this.totalSum = sum;  // Total sum of weights
    }
    
    pickIndex() {
        let target = Math.floor(Math.random() * this.totalSum);
        
        // Linear search for the first index such that prefixSums[index] > target
        for (let i = 0; i < this.prefixSums.length; i++) {
            if (target < this.prefixSums[i]) {
                return i;
            }
        }
        
        return -1;  // In case the linear search doesn't find an index
    }
}
```

#### Complexity
- **Time Complexity**: `O(n)` for initialization (building prefix sum where `n` is the length of the weights array) and `O(n)` for each call to `pickIndex`.
- **Space Complexity**: `O(n)` to store the prefix sum array.

### Approach 2: Prefix Sum and Binary Search

#### Intuition
The linear search might become inefficient for larger inputs. We can use binary search to efficiently find the index, taking advantage of the sorted nature of the prefix sum array. This speeds up finding the appropriate index after computing the prefix sum.

#### Steps
1. As before, compute the prefix sum array.
2. Generate a random number `x` from 0 to the total sum minus 1.
3. Use binary search to find the smallest index `i` such that `prefixSums[i] > x`.
4. Return the found index.

```javascript
class Solution {
    constructor(w) {
        this.prefixSums = [];
        let sum = 0;
        
        // Create the prefix sum array
        for (let weight of w) {
            sum += weight;
            this.prefixSums.push(sum);
        }
        
        this.totalSum = sum;  // Store the total sum of weights
    }
    
    pickIndex() {
        let target = Math.floor(Math.random() * this.totalSum);
        
        // Binary search for the smallest index with prefixSums[index] > target
        let lo = 0, hi = this.prefixSums.length - 1;
        
        while (lo < hi) {
            let mid = Math.floor((lo + hi) / 2);
            
            if (this.prefixSums[mid] > target) {
                hi = mid;
            } else {
                lo = mid + 1;
            }
        }
        
        return lo;
    }
}
```

#### Complexity
- **Time Complexity**: `O(n)` for initialization and `O(log n)` for each `pickIndex` operation due to binary search.
- **Space Complexity**: `O(n)` for storing the prefix sum array.

