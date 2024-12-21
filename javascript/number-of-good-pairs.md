# [LeetCode 1512: Number of Good Pairs](https://leetcode.com/problems/number-of-good-pairs/)

## Approaches:
1. [Brute Force](#approach-1-brute-force)
2. [Optimized with Frequency Count](#approach-2-optimized-with-frequency-count)

---

## Approach 1: Brute Force

### Intuition:
The simplest approach is to check each possible pair `(i, j)` in the array to determine if it forms a "good pair," that is, if `nums[i] == nums[j] and i < j`. This would involve using two nested loops to check all pairs of indices.

### Solution:
```javascript
function numIdenticalPairs(nums) {
    let count = 0;
    // iterate over each element with index i
    for (let i = 0; i < nums.length; i++) {
        // compare it with each subsequent element with index j
        for (let j = i + 1; j < nums.length; j++) {
            // if a good pair is found, increase the count
            if (nums[i] === nums[j]) {
                count++;
            }
        }
    }
    return count;
}
```

### Time Complexity:
- **O(n^2)**: We have a nested loop, each running approximately `n/2` times.
  
### Space Complexity:
- **O(1)**: No extra space is used other than for the count variable.

---

## Approach 2: Optimized with Frequency Count

### Intuition:
Instead of checking each pair individually, we can count the frequency of each number in the array. If a number appears `freq` times, the number of good pairs it forms is `freq * (freq - 1) / 2`, which is the combination formula for selecting 2 items from `freq` items.

### Solution:
```javascript
function numIdenticalPairs(nums) {
    let count = 0;
    let freqMap = {};
    // iterate through each number in the array
    for (let num of nums) {
        // if num has appeared before, it can form a new pair with all its previous occurences
        if (freqMap[num]) {
            count += freqMap[num];
            freqMap[num]++;
        } else {
            // first occurrence of the number; initialize its frequency
            freqMap[num] = 1;
        }
    }
    return count;
}
```

### Time Complexity:
- **O(n)**: We make a single pass through `nums`.

### Space Complexity:
- **O(n)**: In the worst case, all elements are different, and we store each one in the hash map.

