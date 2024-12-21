# [Leetcode 1525: Number of Good Ways to Split a String](https://leetcode.com/problems/number-of-good-ways-to-split-a-string/)

### Approaches:
1. [Brute Force](#brute-force)
2. [Optimized Prefix and Suffix Arrays](#optimized-prefix-and-suffix-arrays)

## Approach 1: Brute Force

### Intuition
The brute force approach is pretty straightforward: we try to split the string at every possible index and check whether the number of distinct characters on each side of the split is equal. We split the string into two parts iteratively and count the unique characters in each part separately.

### Implementation

```javascript
var numSplits = function(s) {
    const n = s.length;
    let result = 0;

    // Iterate through each index as a potential splitting point
    for (let i = 1; i < n; i++) {
        const leftSet = new Set(s.slice(0, i));
        const rightSet = new Set(s.slice(i));

        // Compare number of distinct characters on both sides
        if (leftSet.size === rightSet.size) {
            result++;
        }
    }

    return result;
};
```

### Complexity Analysis

- **Time Complexity**: O(n^2), where n is the length of the string. This is because for each split point, we create and count the distinct characters in two substrings.
- **Space Complexity**: O(n), needed for the sets to count distinct characters for each potential split point.

---

## Approach 2: Optimized Prefix and Suffix Arrays

### Intuition
To optimize, we avoid recalculating the distinct character count for all the splits repeatedly by using prefix and suffix arrays. 

- **Prefix Array**: Keeps track of the number of distinct characters from the start to each index.
- **Suffix Array**: Keeps track of the number of distinct characters from each index to the end.

By constructing these arrays, we can get the distinct count for any prefix or suffix efficiently.

### Implementation

```javascript
var numSplits = function(s) {
    const n = s.length;
    const leftCount = new Array(n).fill(0);
    const rightCount = new Array(n).fill(0);

    const leftSet = new Set();
    const rightSet = new Set();

    // Build prefix array of distinct character counts
    for (let i = 0; i < n; i++) {
        leftSet.add(s[i]);
        leftCount[i] = leftSet.size;
    }

    // Build suffix array of distinct character counts
    for (let i = n - 1; i >= 0; i--) {
        rightSet.add(s[i]);
        rightCount[i] = rightSet.size;
    }

    let result = 0;

    // Compare distinct character counts at each possible split
    for (let i = 0; i < n - 1; i++) {
        if (leftCount[i] === rightCount[i + 1]) {
            result++;
        }
    }

    return result;
};
```

### Complexity Analysis

- **Time Complexity**: O(n), where n is the length of the string. We iterate through the string a few times to build the prefix and suffix arrays.
- **Space Complexity**: O(n), for storing prefix and suffix arrays.

---

In summary, the first approach is easy to understand but inefficient for large inputs, while the second approach efficiently uses additional data structures for counting distinct characters to achieve optimal performance.

