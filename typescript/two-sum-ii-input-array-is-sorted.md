# [Two Sum II - Input Array Is Sorted](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/)

## Approach Navigation
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Two Pointers](#approach-2-two-pointers)
- [Approach 3: Binary Search](#approach-3-binary-search)

## Approach 1: Brute Force

### Intuition
The simplest approach is to use a nested loop to check every pair of numbers and see if their sum is equal to the target. Even though the array is sorted, this approach doesn't take advantage of that property, resulting in a straightforward but inefficient solution.

### Solution in TypeScript
```typescript
function twoSum(numbers: number[], target: number): number[] {
    // Iterate over each element as the first element of the pair
    for (let i = 0; i < numbers.length; i++) {
        // Iterate over subsequent elements as the second element of the pair
        for (let j = i + 1; j < numbers.length; j++) {
            // Check if the current pair sums up to the target
            if (numbers[i] + numbers[j] === target) {
                // Return 1-based indices as per problem statement
                return [i + 1, j + 1];
            }
        }
    }
    // Return an empty array if no solution is found, though per problem statement, there will always be a solution
    return [];
}
```

### Time and Space Complexity
- **Time Complexity**: O(n^2), where n is the number of elements in the array. This is because we are using two nested loops to iterate over all pairs.
- **Space Complexity**: O(1), since we are only using a constant amount of extra space.

## Approach 2: Two Pointers

### Intuition
We can optimize the problem using the two-pointer technique, leveraging the sorted nature of the array. By using two pointers, one starting at the beginning (left) and one at the end (right) of the array, we can adjust their positions based on the comparison between the sum of the elements at these pointers and the target sum.

### Solution in TypeScript
```typescript
function twoSum(numbers: number[], target: number): number[] {
    // Initialize two pointers
    let left = 0;
    let right = numbers.length - 1;
    
    // Iterate while the left pointer is less than the right pointer
    while (left < right) {
        const sum = numbers[left] + numbers[right];
        
        // Check if sum of elements at both pointers is equal to target
        if (sum === target) {
            // Return 1-based indices as per problem statement
            return [left + 1, right + 1];
        } else if (sum < target) {
            // Move left pointer to increase the sum
            left++;
        } else {
            // Move right pointer to decrease the sum
            right--;
        }
    }
    
    return [];
}
```

### Time and Space Complexity
- **Time Complexity**: O(n), where n is the number of elements in the array. We traverse the array just once with two pointers.
- **Space Complexity**: O(1), since we are only using a constant amount of extra space.

## Approach 3: Binary Search

### Intuition
Similar to the two-pointer approach, but using the sorted array property more explicitly by performing a binary search for each element. This involves fixing one number and using binary search to find if the complement (target - numbers[i]) exists in the rest of the array.

### Solution in TypeScript
```typescript
function twoSum(numbers: number[], target: number): number[] {
    // Iterate over each element to fix one number
    for (let i = 0; i < numbers.length; i++) {
        // Calculate the complement we need to find
        const complement = target - numbers[i];
        // Perform binary search for the complement in the rest of the array
        let low = i + 1;
        let high = numbers.length - 1;
        
        while (low <= high) {
            const mid = Math.floor((low + high) / 2);
            if (numbers[mid] === complement) {
                // Return 1-based indices as per problem statement
                return [i + 1, mid + 1];
            } else if (numbers[mid] < complement) {
                low = mid + 1;
            } else {
                high = mid - 1;
            }
        }
    }
    return [];
}
```

### Time and Space Complexity
- **Time Complexity**: O(n log n), due to looping through each element and performing a binary search which is logarithmic.
- **Space Complexity**: O(1), since only a constant amount of extra space is used.

