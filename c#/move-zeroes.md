[Leetcode Problem 283: Move Zeroes](https://leetcode.com/problems/move-zeroes/)

### Approaches
1. [Naive Approach: Using Extra Space](#naive-approach-using-extra-space)
2. [Optimal In-Place Approach: Using Two Pointers](#optimal-in-place-approach-using-two-pointers)

---

### Naive Approach: Using Extra Space

**Intuition**:
The simplest way to tackle this problem is to use an additional array to separate zero and non-zero elements. The idea is to first collect all non-zero elements and then append zero elements.

- Iterate through the array and add all non-zero elements to a new list.
- Append the number of zeros in the tail of the list.
- Copy back the elements from the list to the original array.

**Code**:
```csharp
public void MoveZeroes(int[] nums) {
    // Create a list to store non-zero elements.
    List<int> nonZeroElements = new List<int>();
    
    // Traverse the array and collect all non-zero numbers.
    foreach (int num in nums) {
        if (num != 0) {
            nonZeroElements.Add(num);
        }
    }
    
    // The count of non-zeros we have found so far.
    int nonZeroCount = nonZeroElements.Count;
    
    // Place non-zero elements in the original array.
    for (int i = 0; i < nonZeroCount; i++) {
        nums[i] = nonZeroElements[i];
    }
    
    // Fill remaining positions in the array with zeros.
    for (int i = nonZeroCount; i < nums.Length; i++) {
        nums[i] = 0;
    }
}
```

**Time Complexity**: O(n), where n is the number of elements in the array, as we iterate through the array twice.
**Space Complexity**: O(n), due to the additional space used for the non-zero elements.

---

### Optimal In-Place Approach: Using Two Pointers

**Intuition**:
The optimal solution involves modifying the array in place using two pointers. The goal is to keep all non-zero elements in their relative order without using extra space. 

- Use one pointer (`lastNonZeroFoundAt`) to point where the next non-zero element should go.
- Traverse the array with another pointer (`current`).
- Each time a zero element is encountered, simply skip it.
- When a non-zero item is found, place it at the position indicated by `lastNonZeroFoundAt` and increment this pointer.

**Code**:
```csharp
public void MoveZeroes(int[] nums) {
    int lastNonZeroFoundAt = 0; // This pointer tracks where to place next non-zero element.
    
    // Traverse through the array with the `current` pointer.
    for (int current = 0; current < nums.Length; current++) {
        // If the number is non-zero, we need to move it to the `lastNonZeroFoundAt` index.
        if (nums[current] != 0) {
            // Swap elements at `lastNonZeroFoundAt` and `current` pointers.
            int temp = nums[lastNonZeroFoundAt];
            nums[lastNonZeroFoundAt] = nums[current];
            nums[current] = temp;
            
            // Increment `lastNonZeroFoundAt`.
            lastNonZeroFoundAt++;
        }
    }
}
```

**Time Complexity**: O(n), where n is the number of elements in the array, as we perform a single pass through the array.
**Space Complexity**: O(1), since we use only constant extra space (only a few additional variables).

---

By using the optimal in-place approach, you can solve the problem efficiently without any extra space overhead, which is often preferable in lower-level systems or with a large dataset.

