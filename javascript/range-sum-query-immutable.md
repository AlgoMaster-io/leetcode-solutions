# [Leetcode Problem 303: Range Sum Query - Immutable](https://leetcode.com/problems/range-sum-query-immutable/)

## Approaches
1. [Naive Approach](#naive-approach)
2. [Prefix Sum Approach](#prefix-sum-approach)

### Naive Approach

#### Intuition
The simplest method to solve the problem is to calculate the sum of the elements between the two given indices for every query. This involves iterating through the elements between the given indices each time a query is made. This approach is intuitive but inefficient for a large number of queries since it performs the same computation repeatedly.

#### Code
```javascript
class NumArray {
    constructor(nums) {
        this.nums = nums;
    }
    
    sumRange(left, right) {
        let sum = 0;
        // Loop over from left to right indices as specified in the query
        for (let i = left; i <= right; i++) {
            // Accumulate sum for this query range
            sum += this.nums[i];
        }
        return sum;
    }
}

// Example usage:
const numArray = new NumArray([-2, 0, 3, -5, 2, -1]);
console.log(numArray.sumRange(0, 2)); // 1
console.log(numArray.sumRange(2, 5)); // -1
console.log(numArray.sumRange(0, 5)); // -3
```

#### Complexity Analysis
- **Time Complexity**: O(n) per query, where n is the number of elements between `left` and `right`. Each `sumRange` query requires iterating over the range of elements.
- **Space Complexity**: O(1), as we do not use any additional data structures that grow with input size other than the space to store the `nums` array and a sum variable.

### Prefix Sum Approach

#### Intuition
To optimize the sum computation for each query, we can leverage precomputation using a prefix sum array. The idea is to preprocess the array to calculate and store cumulative sums such that the sum of any subarray can be obtained in constant time. The prefix sum array at index `i` contains the sum of the array elements from the start up to that index.

#### Code
```javascript
class NumArray {
    constructor(nums) {
        // Initialize prefixSums array with one extra space for ease of use
        this.prefixSums = new Array(nums.length + 1).fill(0);
        // Compute prefix sums
        for (let i = 0; i < nums.length; i++) {
            this.prefixSums[i + 1] = this.prefixSums[i] + nums[i];
        }
    }
    
    sumRange(left, right) {
        // Return the sum between left and right indices using the prefix sum array
        // Subtract the cumulative sum up to (left - 1) from cumulative sum up to right.
        return this.prefixSums[right + 1] - this.prefixSums[left];
    }
}

// Example usage:
const numArray = new NumArray([-2, 0, 3, -5, 2, -1]);
console.log(numArray.sumRange(0, 2)); // 1
console.log(numArray.sumRange(2, 5)); // -1
console.log(numArray.sumRange(0, 5)); // -3
```

#### Complexity Analysis
- **Time Complexity**: 
  - Preprocessing: O(n), where n is the length of the input array for constructing the prefix sum array.
  - Query: O(1), each `sumRange` query is resolved in constant time as it involves only two array accesses and a subtraction.
- **Space Complexity**: O(n), to store the prefix sum array of size n+1.

The prefix sum approach drastically improves the query efficiency by sacrificing some additional space, making it highly effective for handling multiple range sum queries on a static array of numbers.

