# [Leetcode 26: Remove Duplicates from Sorted Array](https://leetcode.com/problems/remove-duplicates-from-sorted-array/)

## Navigation
- [Approach 1: Brute Force Approach](#approach-1-brute-force-approach)
- [Approach 2: Two-Pointer Technique (Optimal)](#approach-2-two-pointer-technique-optimal)

## Approach 1: Brute Force Approach

### Intuition
Since the array is sorted, duplicates will always be adjacent. We could use a simple approach where we iterate over the array and shift non-duplicate elements towards the beginning. This approach would involve creating a new array or list to keep track of unique elements, but this does not leverage the given array, thus violating the problem constraint of using O(1) extra space.

### Algorithm
1. Initialize a list to keep track of unique elements.
2. Iterate over the array, and for each element, check if it is already in the unique list.
3. If not, add it to the list.
4. Copy the elements from the list back to the original array.
5. Return the length of the unique list.

**Note:** This approach doesn't fully comply with the problem requirements, as it makes use of extra space.

### Code
```csharp
public int RemoveDuplicates(int[] nums) {
    if (nums.Length == 0) return 0;
    
    List<int> uniqueElements = new List<int>();
    
    for (int i = 0; i < nums.Length; i++) {
        // If the list is empty or the last element is not the current one
        if (uniqueElements.Count == 0 || uniqueElements[uniqueElements.Count - 1] != nums[i]) {
            uniqueElements.Add(nums[i]);  // Add element to the unique list
        }
    }

    for (int i = 0; i < uniqueElements.Count; i++) {
        nums[i] = uniqueElements[i];  // Copy back to the original array
    }

    return uniqueElements.Count;  // Return the count of unique elements
}
```

### Complexity Analysis
- **Time Complexity:** O(n^2), because checking if a list contains an element can take O(n) time.
- **Space Complexity:** O(n), because we are storing the unique elements in a separate list.

## Approach 2: Two-Pointer Technique (Optimal)

### Intuition
A more space-efficient way to handle this problem is the two-pointer technique, which aligns well with the problem constraints. Since the array is sorted, duplicates are adjacent, we can use one pointer to iterate through the array and a second to mark the position where the next unique element should be placed.

### Algorithm
1. Initialize a pointer `i` for the position of the last unique element, starting at 0.
2. Iterate with a second pointer `j` through the array starting from the second element.
3. For each element at `j`, compare it with the element at `i`.
4. If it's not a duplicate (i.e., `nums[i] != nums[j]`), increment `i` and update `nums[i]` to `nums[j]`.
5. Continue this until the end of the array, after which `i + 1` will represent the number of unique elements.

### Code
```csharp
public int RemoveDuplicates(int[] nums) {
    if (nums.Length == 0) return 0;

    int i = 0;  // Pointer for the position of the last unique element

    for (int j = 1; j < nums.Length; j++) {
        if (nums[i] != nums[j]) {  // Only change position 'i' if a new unique element is found
            i++;
            nums[i] = nums[j];  // Update the next unique position with the new element
        }
    }

    return i + 1;  // Length is 'i + 1' since i is zero-indexed
}
```

### Complexity Analysis
- **Time Complexity:** O(n), where n is the length of the array, as each element is compared exactly once.
- **Space Complexity:** O(1), as we don't use any extra space aside from a few variables.

This approach is optimal and adheres to the problem's constraints, effectively using the structure of the sorted array and efficiently managing duplicates.

