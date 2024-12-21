# [Leetcode 528: Random Pick with Weight](https://leetcode.com/problems/random-pick-with-weight/)

## Approaches
- [Approach 1: Linear Search Using Probability](#approach-1-linear-search-using-probability)
- [Approach 2: Binary Search Using Cumulative Weights (Optimal)](#approach-2-binary-search-using-cumulative-weights-optimal)

## Approach 1: Linear Search Using Probability
### Intuition
The problem requires us to return an index such that the probability of it being picked is proportional to its weight. The naive approach would involve summing the weights to understand the total probability space and then using a linear search to determine which index corresponds to the randomly picked value.

### Steps
1. Sum up all the weights.
2. For each pick, generate a random number within the total weight sum.
3. Iterate over the weights, updating the cumulative weight until the random number is less than this cumulative sum. The current index is the selected one.

### Code
```cpp
class Solution {
public:
    vector<int> weights;
    int totalWeight;

    Solution(vector<int>& w) {
        weights = w;
        totalWeight = accumulate(weights.begin(), weights.end(), 0);  // Calculate total weight
    }
    
    int pickIndex() {
        int randWeight = rand() % totalWeight; // Pick a random number in weight range
        int currentWeight = 0;
        
        // Linear search to find out which index to pick based on the cumulative weight
        for (int i = 0; i < weights.size(); ++i) {
            currentWeight += weights[i];
            if (randWeight < currentWeight) {  // Check if the random number falls in the current index range
                return i;
            }
        }
        return -1; // This should never be reached
    }
};
```

### Complexity Analysis
- **Time Complexity:** `O(N)`, where `N` is the number of weights. Each pick operation requires iterating over the weights.
- **Space Complexity:** `O(1)`, aside from the input weights array, no additional space is used.

## Approach 2: Binary Search Using Cumulative Weights (Optimal)
### Intuition
The linear approach can be optimized by using a cumulative weight array and binary search. By pre-computing the cumulative weight, we convert the search problem into finding the upper bound of a randomly generated number, which can be efficiently resolved using binary search.

### Steps
1. Create a cumulative weight array, where each entry at index `i` represents the total weight from the start up to `i`.
2. For picking, generate a random target within the total weights.
3. Utilize binary search to find the first index where the cumulative weight exceeds the random target.

### Code
```cpp
class Solution {
public:
    vector<int> cumulativeWeights;
    int totalWeight;

    Solution(vector<int>& w) {
        cumulativeWeights.resize(w.size());
        cumulativeWeights[0] = w[0];
        
        // Calculate cumulative weights
        for (int i = 1; i < w.size(); ++i) {
            cumulativeWeights[i] = cumulativeWeights[i - 1] + w[i];
        }
        totalWeight = cumulativeWeights.back(); // Total weight is the last cumulative element
    }
    
    int pickIndex() {
        int target = rand() % totalWeight;
        
        // Binary search to find the correct index
        int low = 0, high = cumulativeWeights.size() - 1;
        while (low < high) {
            int mid = low + (high - low) / 2;
            if (target < cumulativeWeights[mid]) {
                high = mid;  // The target is within this range
            } else {
                low = mid + 1; // Go right since target is greater than cumulativeWeight[mid]
            }
        }
        return low;
    }
};
```

### Complexity Analysis
- **Time Complexity:** `O(log N)`, where `N` is the number of weights due to the binary search.
- **Space Complexity:** `O(N)`, for the cumulative weights array.

