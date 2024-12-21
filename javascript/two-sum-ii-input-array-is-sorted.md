[Leetcode 167: Two Sum II - Input Array Is Sorted](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/)

### Approaches
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Two Pointers](#approach-2-two-pointers)

---

## Approach 1: Brute Force

### Intuition
Since the array is sorted, a basic yet non-optimal approach is to use a nested loop to check all possible pairs, looking for a pair whose elements add up to the target value.

### Code
```javascript
function twoSum(numbers, target) {
    const n = numbers.length;
    for (let i = 0; i < n; i++) {
        for (let j = i + 1; j < n; j++) {
            // Check if the sum of the current pair equals to the target
            if (numbers[i] + numbers[j] === target) {
                return [i + 1, j + 1]; // Return indices in 1-based index
            }
        }
    }
    return [];
}
```

### Complexity Analysis
- **Time Complexity:** \(O(n^2)\) because we iterate over all pairs in the worst case.
- **Space Complexity:** \(O(1)\) because no extra space is used beyond variables.

---

## Approach 2: Two Pointers

### Intuition
In a sorted array, two pointers can efficiently solve the problem by moving one pointer from the start and the other from the end. If the sum of the elements at these pointers is less than the target, we move the left pointer to the right. If the sum is greater than the target, we move the right pointer to the left. We continue this process until we find the pair that equals the target.

### Code
```javascript
function twoSum(numbers, target) {
    let left = 0;
    let right = numbers.length - 1;
    
    while (left < right) {
        const currentSum = numbers[left] + numbers[right];
        
        // Check if the current pair's sum equals to the target
        if (currentSum === target) {
            return [left + 1, right + 1]; // Return indices in 1-based index
        } else if (currentSum < target) {
            // If current sum is less, move the left pointer to the right
            left++;
        } else {
            // If current sum is more, move the right pointer to the left
            right--;
        }
    }
    
    return [];
}
```

### Complexity Analysis
- **Time Complexity:** \(O(n)\) where \(n\) is the length of the array, because each element is visited at most once.
- **Space Complexity:** \(O(1)\) as we use only constant extra space for pointers.

