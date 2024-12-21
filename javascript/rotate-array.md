# [Leetcode 189: Rotate Array](https://leetcode.com/problems/rotate-array/)

## Approaches

1. [Naive approach: Using extra space](#naive-approach)
2. [Cyclic Replacements](#cyclic-replacements)
3. [Reverse Approach (Most Optimal)](#reverse-approach)

## Naive approach: Using extra space

### Intuition
The simplest approach is to use an additional array. We can store the rotated version of the array in this new array, and then copy it back to the original array.

### Steps
1. Create a new array to hold the rotated result.
2. Iterate over the original array and calculate the new position for each element in the rotated array.
3. Copy the rotated result back to the original array.

### Code

```javascript
function rotate(nums, k) {
    let n = nums.length;
    // Create a new array to store rotated elements
    let rotated = new Array(n);
    
    // Place each element in its new position
    for (let i = 0; i < n; i++) {
        rotated[(i + k) % n] = nums[i];
    }
    
    // Copy the rotated elements back to original array
    for (let i = 0; i < n; i++) {
        nums[i] = rotated[i];
    }
}
```

### Complexity Analysis
- **Time Complexity**: O(n), where n is the number of elements in the array. We traverse the array twice.
- **Space Complexity**: O(n), since we're using an additional array of size n.

## Cyclic Replacements

### Intuition
Another approach without using extra space is to use cyclic replacements. We move each number to its correct position using the modulo operation and repeat this process for each cycle of replacements.

### Steps
1. Iterate through each element.
2. For each element, place it in its new correct position and keep replacing the displaced elements cyclically until we return to the starting position.

### Code

```javascript
function rotate(nums, k) {
    let n = nums.length;
    k = k % n;
    let count = 0; // number of elements placed correctly
    
    for (let start = 0; count < n; start++) {
        let current = start;
        let prevNum = nums[start];
        
        do {
            let next = (current + k) % n;
            let temp = nums[next];
            nums[next] = prevNum;
            prevNum = temp;
            current = next;
            count++;
        } while (start !== current);
    }
}
```

### Complexity Analysis
- **Time Complexity**: O(n), since each element is visited once.
- **Space Complexity**: O(1), as we don't use any extra space.

## Reverse Approach (Most Optimal)

### Intuition
The most efficient way to solve this problem is by realizing that the rotation rearrangement can be achieved by reversing parts of the array.

### Steps
1. Reverse the entire array.
2. Reverse the first k elements.
3. Reverse the last n-k elements.

### Code

```javascript
function rotate(nums, k) {
    let n = nums.length;
    k = k % n; // To handle k > n
    
    // Helper function to reverse a portion of the array
    function reverse(start, end) {
        while (start < end) {
            let temp = nums[start];
            nums[start] = nums[end];
            nums[end] = temp;
            start++;
            end--;
        }
    }
    
    // Reverse the entire array
    reverse(0, n - 1);
    // Reverse the first k elements to restore order
    reverse(0, k - 1);
    // Reverse the last n-k elements to restore order
    reverse(k, n - 1);
}
```

### Complexity Analysis
- **Time Complexity**: O(n), since each reverse operation is O(n).
- **Space Complexity**: O(1), as we're modifying the array in place.

