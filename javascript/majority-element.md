## [Leetcode 169: Majority Element](https://leetcode.com/problems/majority-element/)

### Approaches:
1. [HashMap Approach](#hashmap-approach)
2. [Sorting Approach](#sorting-approach)
3. [Boyer-Moore Voting Algorithm](#boyer-moore-voting-algorithm)

---

### HashMap Approach

**Intuition:**

The basic idea is to use a HashMap to count the occurrences of each element in the array. The element with a count greater than `n/2` is the majority element.

**Steps:**
1. Initialize an empty HashMap to store the count of each element.
2. Iterate through the array, and for each element, update its count in the HashMap.
3. As soon as an element’s count becomes greater than `n/2`, return that element.

**Code:**

```javascript
function majorityElement(nums) {
    const countMap = new Map();
    
    for (let num of nums) {
        // Update the count of each number in the map
        countMap.set(num, (countMap.get(num) || 0) + 1);
        
        // Check if any number appears more than n/2 times
        if (countMap.get(num) > Math.floor(nums.length / 2)) {
            return num;
        }
    }
}
```

**Time Complexity:** O(n) - We traverse the array once.
**Space Complexity:** O(n) - We store counts for up to n/2 + 1 different elements.

---

### Sorting Approach

**Intuition:**

By sorting the array, the majority element (which appears more than n/2 times) must be present in the middle of the array.

**Steps:**
1. Sort the array.
2. Return the element at the middle index.

**Code:**

```javascript
function majorityElement(nums) {
    // Sort the array
    nums.sort((a, b) => a - b);
    
    // Return the middle element, which is guaranteed to be the majority element
    return nums[Math.floor(nums.length / 2)];
}
```

**Time Complexity:** O(n log n) - Due to the sort operation.
**Space Complexity:** O(1) - Ignoring the space used by sort.

---

### Boyer-Moore Voting Algorithm

**Intuition:**

This optimal approach involves counting a candidate element against others. If a number is the majority, it will be the last element standing after cancellations.

**Steps:**
1. Initialize `count` as 0 and `candidate` as undefined at the start.
2. Loop through the array:
   - If `count` is 0, set the current number as `candidate` and increment `count`.
   - If the current number is the same as `candidate`, increment `count`.
   - Otherwise, decrement `count`.
3. Return `candidate`, the majority element.

**Code:**

```javascript
function majorityElement(nums) {
    let count = 0;
    let candidate = null;
    
    for (let num of nums) {
        // Assign a new candidate if count drops to zero
        if (count === 0) {
            candidate = num;
        }
        
        // Increment or decrement the count
        count += (num === candidate) ? 1 : -1;
    }
    
    // The remaining candidate is the majority element
    return candidate;
}
```

**Time Complexity:** O(n) - We traverse the array once.
**Space Complexity:** O(1) - Only constant space is used.

---

