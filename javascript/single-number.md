# [Leetcode 136: Single Number](https://leetcode.com/problems/single-number/)

## Approaches:
1. [Brute Force Using Nested Loops](#approach-1-brute-force-using-nested-loops)
2. [Using Sorting](#approach-2-using-sorting)
3. [Using Hash Map](#approach-3-using-hash-map)
4. [XOR Bit Manipulation](#approach-4-xor-bit-manipulation)

---

## Approach 1: Brute Force Using Nested Loops

### Intuition:
In a brute force approach, for each number, we can check if it appears only once in the array by counting its occurrences. This straightforward method helps understand the problem's core logic but is not efficient for large arrays.

### Solution:

```javascript
function singleNumber(nums) {
    // Iterate through each element in nums
    for (let i = 0; i < nums.length; i++) {
        let count = 0;
        // Count how many times nums[i] appears in nums
        for (let j = 0; j < nums.length; j++) {
            if (nums[i] === nums[j]) {
                count++;
            }
        }
        // If nums[i] appears only once, it is our answer
        if (count === 1) {
            return nums[i];
        }
    }
    return -1; // if no single element is found, though problem guarantees one exists
}
```

**Time Complexity:** O(n^2)  
**Space Complexity:** O(1)

---

## Approach 2: Using Sorting

### Intuition:
By sorting the array, all identical elements will be adjacent. We can then iterate through the sorted list and find the single element by checking if the current element is different from its neighbors.

### Solution:

```javascript
function singleNumber(nums) {
    // Sort the array
    nums.sort((a, b) => a - b);
    // Check each element with its adjacent ones
    for (let i = 0; i < nums.length; i += 2) {
        // If this is the last element or not equal to the next one
        if (i === nums.length - 1 || nums[i] !== nums[i + 1]) {
            return nums[i];
        }
    }
    return -1; // if no single element is found, though problem guarantees one exists
}
```

**Time Complexity:** O(n log n) due to sorting  
**Space Complexity:** O(1)

---

## Approach 3: Using Hash Map

### Intuition:
Using a hash map (or object in JavaScript), we can efficiently count occurrences of each number in the array. The number with a count of one will be our answer.

### Solution:

```javascript
function singleNumber(nums) {
    const countMap = {};
    // Populate the hash map with counts
    for (let num of nums) {
        if (countMap[num]) {
            countMap[num]++;
        } else {
            countMap[num] = 1;
        }
    }
    // Find the number with count of one
    for (let num in countMap) {
        if (countMap[num] === 1) {
            return parseInt(num);
        }
    }
    return -1; // if no single element is found, though problem guarantees one exists
}
```

**Time Complexity:** O(n)  
**Space Complexity:** O(n)

---

## Approach 4: XOR Bit Manipulation

### Intuition:
Using the property of XOR: 
- a ⊕ a = 0 
- a ⊕ 0 = a
If we XOR all numbers, equal numbers will cancel each other (result in 0), and only the single number will remain as 0 ⊕ single = single.

### Solution:

```javascript
function singleNumber(nums) {
    let result = 0;
    // XOR all numbers - duplicates will cancel out
    for (let num of nums) {
        result ^= num; // XOR operation
    }
    return result;
}
```

**Time Complexity:** O(n)  
**Space Complexity:** O(1)

The XOR approach is the most efficient here since it uses a linear time complexity and constant space, making it optimal for this problem.

