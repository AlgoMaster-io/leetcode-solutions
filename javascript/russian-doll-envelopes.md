# [Leetcode 354: Russian Doll Envelopes](https://leetcode.com/problems/russian-doll-envelopes/)

## Approaches
1. [Brute Force Approach](#brute-force-approach)
2. [Dynamic Programming Approach](#dynamic-programming-approach)
3. [Optimal Approach with Sorting and Longest Increasing Subsequence](#optimal-approach-with-sorting-and-longest-increasing-subsequence)

---

## Brute Force Approach

### Intuition
The brute force approach involves checking every combination of envelopes to see if they can fit inside one another. We can do this by trying every possible subsequence of envelopes and checking if one can fit inside the next.

### Implementation

```javascript
function canEnvelopeFit(env1, env2) {
    return env1[0] < env2[0] && env1[1] < env2[1];
}

var maxEnvelopes = function(envelopes) {
    if(envelopes.length === 0) return 0;
    
    let maxCount = 1;
    const n = envelopes.length;
    
    function helper(index, previous) {
        if(index === n) return 0;

        // Take the current envelope
        let taken = 0;
        if(previous === -1 || canEnvelopeFit(envelopes[previous], envelopes[index])) {
            taken = 1 + helper(index + 1, index);
        }
        
        // Skip the current envelope
        let notTaken = helper(index + 1, previous);
        
        return Math.max(taken, notTaken);
    }
    
    return helper(0, -1);
};
```

### Complexity
- **Time Complexity:** \(O(2^n)\), as we are exploring all possible subsets of envelopes.
- **Space Complexity:** \(O(n)\) due to the recursion stack.

---

## Dynamic Programming Approach

### Intuition
This approach builds upon the idea of the longest increasing subsequence within a particular order of envelopes. We can first sort the envelopes by width, and for each envelope, determine the longest sequence ending at that envelope.

### Implementation

```javascript
var maxEnvelopes = function(envelopes) {
    if(envelopes.length === 0) return 0;

    envelopes.sort((a, b) => a[0] - b[0] || a[1] - b[1]);
    const n = envelopes.length;
    const dp = Array(n).fill(1);
    let maxCount = 1;
    
    for(let i = 1; i < n; i++) {
        for(let j = 0; j < i; j++) {
            if(envelopes[j][0] < envelopes[i][0] && envelopes[j][1] < envelopes[i][1]) {
                dp[i] = Math.max(dp[i], dp[j] + 1);
            }
        }
        maxCount = Math.max(maxCount, dp[i]);
    }
    
    return maxCount;
};
```

### Complexity
- **Time Complexity:** \(O(n^2)\), due to the double loop for comparison.
- **Space Complexity:** \(O(n)\), for the dp array storing sequence lengths.

---

## Optimal Approach with Sorting and Longest Increasing Subsequence

### Intuition
The optimal solution leverages the sorting and LIS (Longest Increasing Subsequence) technique. By sorting the envelopes by width in ascending order and by height in descending order when widths are the same, we can apply LIS on the heights to find the maximum sequence of envelopes.

### Implementation

```javascript
var maxEnvelopes = function(envelopes) {
    if(envelopes.length === 0) return 0;

    envelopes.sort((a, b) => {
        if (a[0] === b[0]) {
            return b[1] - a[1];
        }
        return a[0] - b[0];
    });
    
    const heights = envelopes.map(env => env[1]);
    
    const lis = [];
    for (let height of heights) {
        let left = 0, right = lis.length;
        
        while (left < right) {
            let mid = left + Math.floor((right - left) / 2);
            if (lis[mid] < height) {
                left = mid + 1;
            } else {
                right = mid;
            }
        }
        
        if (left >= lis.length) {
            lis.push(height);
        } else {
            lis[left] = height;
        }
    }
    
    return lis.length;
};
```

### Complexity
- **Time Complexity:** \(O(n \log n)\), due to sorting the envelopes and binary search for LIS.
- **Space Complexity:** \(O(n)\), for storing the heights of the envelopes.

---

