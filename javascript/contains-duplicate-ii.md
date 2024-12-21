# [Leetcode 219: Contains Duplicate II](https://leetcode.com/problems/contains-duplicate-ii/)

## Approaches

1. [Brute Force - Double Loop](#brute-force)
2. [Hash Map for Index Tracking](#hash-map)

---

### Approach 1: Brute Force - Double Loop

**Intuition**: 
The simplest way to solve the problem is by using a double loop. For each element, we check all the subsequent elements within a range of `k` to see if there is a duplicate. This guarantees that we find duplicates only within the required distance. 

**Code**:
```javascript
function containsNearbyDuplicate(nums, k) {
    // Loop over each element in the array
    for (let i = 0; i < nums.length; i++) {
        // For the current element, check the next k elements
        for (let j = i + 1; j <= i + k && j < nums.length; j++) {
            // If a duplicate is found within the range, return true
            if (nums[i] === nums[j]) {
                return true;
            }
        }
    }
    // If no duplicates are found within k distance, return false
    return false;
}
```

**Time Complexity**: \(O(n \times k)\), where \(n\) is the number of elements in the array. In the worst case, we check each element with the next \(k\) elements.

**Space Complexity**: \(O(1)\), as no additional space is used that scales with input size.

---

### Approach 2: Hash Map for Index Tracking

**Intuition**:
The brute force solution can be optimized by using a hash map to track the indices of elements. The key idea is to store the last seen index of each element and continuously check the difference between the current index and the stored index. If the difference is less than or equal to \(k\), a duplicate within the required distance is found.

**Code**:
```javascript
function containsNearbyDuplicate(nums, k) {
    // Create a hash map to store the last index of each number
    const indexMap = new Map();
    
    // Loop over each element in the array
    for (let i = 0; i < nums.length; i++) {
        // If the current number was seen before
        if (indexMap.has(nums[i])) {
            // Calculate the difference between current and last seen index
            const prevIndex = indexMap.get(nums[i]);
            if (i - prevIndex <= k) {
                // If the difference is within k, return true
                return true;
            }
        }
        
        // Store the current index of the element in the map
        indexMap.set(nums[i], i);
    }
    
    // If no duplicates are found within k distance, return false
    return false;
}
```

**Time Complexity**: \(O(n)\), as we only loop through the array once. Lookup and insert operations in a hash map are average \(O(1)\).

**Space Complexity**: \(O(n)\), where \(n\) is the number of elements in the array. In the worst case, we may store every element in the hash map.

