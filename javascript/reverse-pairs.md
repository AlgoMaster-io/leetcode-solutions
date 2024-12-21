[Leetcode Problem 493: Reverse Pairs](https://leetcode.com/problems/reverse-pairs/)

### Approaches:
- [Brute Force](#brute-force)
- [Merge Sort](#merge-sort)

---

### Brute Force

The brute force approach involves checking each pair `(i, j)` to see if `i < j` and `nums[i] > 2 * nums[j]`. This can be done using nested loops.

#### Intuition:

We need to check for all pairs `(i, j)` if the condition `nums[i] > 2 * nums[j]` holds true. The simplest way to find this is by iterating over each element and checking against all subsequent elements, which will result in a time complexity of \(O(n^2)\).

#### Implementation:

```javascript
function reversePairs(nums) {
    let count = 0;
    
    // Iterate over each element as the first element of the pair
    for (let i = 0; i < nums.length; i++) {
        // Check all subsequent elements for valid pairs
        for (let j = i + 1; j < nums.length; j++) {
            // If nums[i] > 2 * nums[j], count it as a reverse pair
            if (nums[i] > 2 * nums[j]) {
                count++;
            }
        }
    }
    
    return count;
}
```

- **Time Complexity**: \(O(n^2)\) due to the two nested loops.
- **Space Complexity**: \(O(1)\) as no additional space is used.

--- 

### Merge Sort

This approach leverages a modified merge sort to count the reverse pairs while sorting. The key idea is to count the pairs as each merge step is executed.

#### Intuition:

The problem of counting reverse pairs can be efficiently solved by modifying the merge sort algorithm. While merging two halves, if an element in the left half is greater than twice the elements in the right half, then all subsequent elements in the left half will also satisfy this condition, since the array is sorted. This allows us to count the pairs without individually comparing each element after sorting the halves.

#### Implementation:

```javascript
function reversePairs(nums) {
    // Helper function to count reverse pairs using merge sort
    function mergeSortAndCount(nums, start, end) {
        if (start >= end) return 0; // Base case
        
        const mid = Math.floor((start + end) / 2);
        let count = mergeSortAndCount(nums, start, mid) 
                  + mergeSortAndCount(nums, mid + 1, end);
        
        // Count reverse pairs across the two halves
        let j = mid + 1;
        for (let i = start; i <= mid; i++) {
            while (j <= end && nums[i] > 2 * nums[j]) {
                j++;
            }
            count += (j - (mid + 1));
        }
        
        // Merge the two halves
        let leftArray = nums.slice(start, mid + 1);
        let rightArray = nums.slice(mid + 1, end + 1);
        let i = 0, k = start, l = 0;
        
        while (i < leftArray.length && l < rightArray.length) {
            if (leftArray[i] <= rightArray[l]) {
                nums[k++] = leftArray[i++];
            } else {
                nums[k++] = rightArray[l++];
            }
        }
        
        while (i < leftArray.length) nums[k++] = leftArray[i++];
        while (l < rightArray.length) nums[k++] = rightArray[l++];
        
        return count;
    }
    
    return mergeSortAndCount(nums, 0, nums.length - 1);
}
```

- **Time Complexity**: \(O(n \log n)\) due to the merge sort.
- **Space Complexity**: \(O(n)\) due to the arrays used during the merge process.

This approach is more efficient and optimal for larger arrays due to divide and conquer.

