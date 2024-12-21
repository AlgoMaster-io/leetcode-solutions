[Median of Two Sorted Arrays - LeetCode](https://leetcode.com/problems/median-of-two-sorted-arrays/)

## Solutions
- [Approach 1: Merge and Sort](#approach-1-merge-and-sort)
- [Approach 2: Binary Search](#approach-2-binary-search)

### Approach 1: Merge and Sort

#### Intuition:
The simplest way to find the median of two sorted arrays is to merge the arrays and then find the median. This straightforward approach utilizes the fact that we can directly access an element using its index once the two arrays are merged into a single sorted array.

#### Steps:
1. Merge the two arrays into a single new array.
2. Sort the merged array.
3. Calculate the median by accessing the middle element(s) of the sorted array.

#### Code:
```javascript
function findMedianSortedArrays(nums1, nums2) {
    // Step 1: Concatenate the two arrays
    let mergedArray = nums1.concat(nums2);
    // Step 2: Sort the merged array
    mergedArray.sort((a, b) => a - b);
    // Step 3: Calculate the median
    const n = mergedArray.length;
    if (n % 2 === 1) {
        // If odd, return the middle element
        return mergedArray[Math.floor(n / 2)];
    } else {
        // If even, return the average of the two middle elements
        return (mergedArray[Math.floor(n / 2) - 1] + mergedArray[Math.floor(n / 2)]) / 2;
    }
}
```

#### Complexity Analysis:
- **Time Complexity**: O((m + n) log(m + n)) due to the sorting step, where `m` and `n` are the lengths of the two arrays.
- **Space Complexity**: O(m + n) to store the merged array.

### Approach 2: Binary Search

#### Intuition:
A more efficient approach is to utilize binary search to find the correct partition between the two arrays. The goal is to identify two partitions such that the left parts from both arrays contain smaller elements than the right parts, allowing us to calculate the median directly.

#### Steps:
1. Always perform binary search on the smaller array for efficiency.
2. Use binary search to find a partition of both arrays, dividing them into left and right parts.
3. Adjust the partition using conditions to maintain the order that the maximum of the left part is less than or equal to the minimum of the right part.
4. Calculate the median based on the total length (odd/even).

#### Code:
```javascript
function findMedianSortedArrays(nums1, nums2) {
    // Ensure nums1 is the smaller array
    if (nums1.length > nums2.length) {
        return findMedianSortedArrays(nums2, nums1);
    }

    let x = nums1.length;
    let y = nums2.length;
    
    let low = 0;
    let high = x;
    
    while (low <= high) {
        const partitionX = Math.floor((low + high) / 2);
        const partitionY = Math.floor((x + y + 1) / 2) - partitionX;
        
        const maxX = (partitionX === 0) ? -Infinity : nums1[partitionX - 1];
        const maxY = (partitionY === 0) ? -Infinity : nums2[partitionY - 1];
        
        const minX = (partitionX === x) ? Infinity : nums1[partitionX];
        const minY = (partitionY === y) ? Infinity : nums2[partitionY];
        
        if (maxX <= minY && maxY <= minX) {
            // We found the correct partition
            if ((x + y) % 2 === 0) {
                return (Math.max(maxX, maxY) + Math.min(minX, minY)) / 2;
            } else {
                return Math.max(maxX, maxY);
            }
        } else if (maxX > minY) {
            // Move towards the left in nums1
            high = partitionX - 1;
        } else {
            // Move towards the right in nums1
            low = partitionX + 1;
        }
    }

    throw new Error("Input arrays are not sorted");
}
```

#### Complexity Analysis:
- **Time Complexity**: O(log(min(m, n))), where `m` is the length of nums1 and `n` is the length of nums2, as we run binary search on the smaller array.
- **Space Complexity**: O(1) since we are only using a constant amount of extra space.

