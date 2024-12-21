# [189. Rotate Array](https://leetcode.com/problems/rotate-array/)

## Approaches
1. [Use Extra Array](#use-extra-array)
2. [Cyclic Replacements](#cyclic-replacements)
3. [Reverse Array](#reverse-array)

### Use Extra Array

#### Intuition
The simplest way to solve the problem is to create an extra array to store the rotated version of the array. We can determine the new position of each element by calculating `(i + k) % n`, where `i` is the current index and `n` is the total length of the array. Then, we copy each element from the original array to its new position in the extra array.

#### Code
```csharp
public void Rotate(int[] nums, int k) {
    // Calculate the effective rotations needed as k may be greater than length of array
    k = k % nums.Length;

    // Create a new array to hold rotated elements
    int[] rotated = new int[nums.Length];

    // Place elements in their new positions
    for (int i = 0; i < nums.Length; i++) {
        // Calculate the new index after rotation using modulo
        int newIndex = (i + k) % nums.Length;
        rotated[newIndex] = nums[i];
    }
    
    // Copy rotated elements back into original array
    for (int i = 0; i < nums.Length; i++) {
        nums[i] = rotated[i];
    }
}
```

#### Complexity Analysis
- **Time Complexity**: O(n), where n is the length of the array, because we iterate over the array twice.
- **Space Complexity**: O(n), as we use an extra array to store the rotated version.

### Cyclic Replacements

#### Intuition
We can rotate the array in-place by using a cyclic replacement. The idea is to replace elements in cycles in the array, with each loop of cycle handling a complete set of replacements for one cycle of movement for elements.

#### Code
```csharp
public void Rotate(int[] nums, int k) {
    int n = nums.Length;
    k = k % n;
    int count = 0; // Count of elements we have moved
    
    for (int start = 0; count < n; start++) {
        int current = start;
        int prev = nums[start];
        
        do {
            int next = (current + k) % n;
            int temp = nums[next];
            nums[next] = prev;
            prev = temp;
            current = next;
            count++;
        } while (start != current); // Cycle is complete when we return to the starting index
    }
}
```

#### Complexity Analysis
- **Time Complexity**: O(n), where n is the length of the array as each element is visited once.
- **Space Complexity**: O(1), no extra space is used other than a few auxiliary variables.

### Reverse Array

#### Intuition
By reversing parts of the array, we can achieve the rotation without any extra space apart from a few variables. First, reverse the whole array. Then, reverse the first `k` elements followed by reversing the rest to achieve the required order.

#### Code
```csharp
public void Rotate(int[] nums, int k) {
    k = k % nums.Length;
    Reverse(nums, 0, nums.Length - 1);    // Reverse the entire array
    Reverse(nums, 0, k - 1);              // Reverse the first k elements
    Reverse(nums, k, nums.Length - 1);    // Reverse the rest of the elements
}

private void Reverse(int[] nums, int start, int end) {
    while (start < end) {
        int temp = nums[start];
        nums[start] = nums[end];
        nums[end] = temp;
        start++;
        end--;
    }
}
```

#### Complexity Analysis
- **Time Complexity**: O(n), where n is the length of the array because of reversing the array thrice.
- **Space Complexity**: O(1), no extra space apart from a few variables used for swapping.
   
This approach is often considered elegant for its space efficiency while providing a clear mechanism for rotation through reversals.

