# [202. Happy Number](https://leetcode.com/problems/happy-number/)

## Approach 1: Using Set to Detect Cycles

### Solution
```javascript
// Time Complexity: O(log(n))
// Space Complexity: O(log(n))

var isHappy = function(n) {
    const seen = new Set();

    while (n !== 1 && !seen.has(n)) {
        seen.add(n);
        n = getNextNumber(n);
    }

    return n === 1;
};

var getNextNumber = function(n) {
    let sum = 0;

    while (n > 0) {
        let digit = n % 10;
        sum += digit * digit;
        n = Math.floor(n / 10);
    }

    return sum;
};
```

## Approach 2: Fast and Slow Pointers (Optimal for Cycle Detection)

### Solution
```javascript
// Time Complexity: O(log(n))
// Space Complexity: O(1)

var isHappy = function(n) {
    let slow = n;
    let fast = getNextNumber(n);

    while (fast !== 1 && slow !== fast) {
        slow = getNextNumber(slow);
        fast = getNextNumber(getNextNumber(fast));
    }

    return fast === 1;
};

var getNextNumber = function(n) {
    let sum = 0;

    while (n > 0) {
        let digit = n % 10;
        sum += digit * digit;
        n = Math.floor(n / 10);
    }

    return sum;
};
```

