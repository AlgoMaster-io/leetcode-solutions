# [1512. Number of Good Pairs](https://leetcode.com/problems/number-of-good-pairs/)

## Approach 1: Brute Force

### Solution
```javascript
// Time Complexity: O(n^2)
// Space Complexity: O(1)
var numIdenticalPairs = function(nums) {
    let count = 0;

    // Compare every pair of elements
    for (let i = 0; i < nums.length; i++) {
        for (let j = i + 1; j < nums.length; j++) {
            if (nums[i] === nums[j]) {
                count++; // Increment count if pair is "good"
            }
        }
    }

    return count;
};
```

## Approach 2: HashMap for Counting Frequencies

### Solution
```javascript
// Time Complexity: O(n)
// Space Complexity: O(n)
var numIdenticalPairs = function(nums) {
    const countMap = new Map();
    let count = 0;

    // Count the occurrences of each number
    for (let num of nums) {
        if (countMap.has(num)) {
            count += countMap.get(num); // Add the current count of this number
        }
        countMap.set(num, (countMap.get(num) || 0) + 1); // Increment the count
    }

    return count;
};
```

## Approach 3: Frequency Array (Optimized for Small Range)

### Solution
```javascript
// Time Complexity: O(n)
// Space Complexity: O(1) (assuming the range of numbers is limited)
var numIdenticalPairs = function(nums) {
    const freq = new Array(101).fill(0); // Frequency array for numbers in range [1, 100]
    let count = 0;

    // Count pairs using frequency
    for (let num of nums) {
        count += freq[num]; // Add the number of pairs that can be formed
        freq[num]++; // Increment the frequency of the current number
    }

    return count;
};
```

