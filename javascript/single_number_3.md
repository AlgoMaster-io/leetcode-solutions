# 260. [Single Number III](https://leetcode.com/problems/single-number-iii/)

## Approach 1: HashMap for Counting Frequencies

### Solution
```javascript
// Time Complexity: O(n)
// Space Complexity: O(n)
function singleNumber(nums) {
    const countMap = new Map();
    
    // Count the frequency of each number
    nums.forEach(num => {
        countMap.set(num, (countMap.get(num) || 0) + 1);
    });
    
    const result = [];
    
    // Find the numbers with frequency 1
    countMap.forEach((count, num) => {
        if (count === 1) {
            result.push(num);
        }
    });
    
    return result;
}
```

## Approach 2: Bit Manipulation

### Solution
```javascript
// Time Complexity: O(n)
// Space Complexity: O(1)
function singleNumber(nums) {
    let xor = 0;
    
    // XOR all numbers to get xor of the two unique numbers
    nums.forEach(num => {
        xor ^= num;
    });
    
    // Find a set bit (rightmost) in xor to separate the numbers
    const diff = xor & -xor;
    
    const result = [0, 0];
    
    // Separate the numbers into two groups and XOR them respectively
    nums.forEach(num => {
        if ((num & diff) === 0) {
            result[0] ^= num; // XOR for first group
        } else {
            result[1] ^= num; // XOR for second group
        }
    });
    
    return result;
}
```

