# [Leetcode 1512: Number of Good Pairs](https://leetcode.com/problems/number-of-good-pairs/)

## Approaches

1. [Brute-Force Approach](#brute-force-approach)
2. [Frequency Mapping Approach](#frequency-mapping-approach)

### Brute-Force Approach

#### Intuition

The problem requires us to find pairs `(i, j)` such that `nums[i] == nums[j]` and `i < j`. A straightforward way to solve this is by checking all possible pairs using two nested loops. For every element, we compare it with the subsequent elements in the array to count the number of good pairs.

#### Implementation

```cpp
class Solution {
public:
    int numIdenticalPairs(vector<int>& nums) {
        int count = 0; // Initialize a counter for good pairs
        
        // Iterate over each element with index i
        for(int i = 0; i < nums.size(); i++) {
            // Compare with each subsequent element with index j
            for(int j = i + 1; j < nums.size(); j++) {
                // If a good pair is found, increment the counter
                if(nums[i] == nums[j]) {
                    count++;
                }
            }
        }
        
        return count; // Return the total number of good pairs
    }
};
```

#### Complexity Analysis

- **Time Complexity:** O(n^2) - We use a nested loop to check all pairs, leading to quadratic time complexity.
- **Space Complexity:** O(1) - We do not allocate any extra space for computation.

### Frequency Mapping Approach

#### Intuition

The idea here is to use a hash map to store the frequency of each number. A pair `(i, j)` is considered good for a number `x` if `x` appeared before. If a number appeared `f` times previously, we can form `f` good pairs with the current occurrence.

For each number, use its frequency to determine how many good pairs can be formed with it.

#### Implementation

```cpp
class Solution {
public:
    int numIdenticalPairs(vector<int>& nums) {
        unordered_map<int, int> freqMap;
        int count = 0; // Initialize a counter for good pairs
        
        // Iterate through each number in the array
        for(int num : nums) {
            if(freqMap.find(num) != freqMap.end()) {
                // If the number has been seen before, it can form a 'good pair' with each of its previous occurrences
                count += freqMap[num]; 
            }
            // Increment the frequency of the current number
            freqMap[num]++;
        }
        
        return count; // Return the total number of good pairs
    }
};
```

#### Complexity Analysis

- **Time Complexity:** O(n) - We iterate over the list once to populate the frequency map and calculate the pairs.
- **Space Complexity:** O(n) - We use a hash map to store frequency counts for every unique number in the array.

These solutions provide a detailed understanding of how to approach the problem of finding good pairs with varying efficiency levels. The frequency mapping method is optimal in both time and space for this particular problem.

