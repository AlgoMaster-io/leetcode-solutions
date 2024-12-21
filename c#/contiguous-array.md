# [LeetCode 525: Contiguous Array](https://leetcode.com/problems/contiguous-array/)

## Approaches
1. [Brute Force Approach](#approach-1-brute-force-approach)
2. [HashMap Approach](#approach-2-hashmap-approach)

---

## Approach 1: Brute Force Approach

### Intuition
The brute force approach involves checking every possible subarray of the given array. Calculate the number of zeros and ones in each subarray to determine if it's balanced (i.e., an equal number of 0s and 1s). This approach isn't efficient, as it checks each subarray independently with no consideration of previously computed results.

### Steps
1. Iterate through each possible starting index `i` of the subarray.
2. For each `i`, iterate through possible ending indices `j` (`j >= i`).
3. Calculate the number of `0`s and `1`s in the subarray `nums[i...j]`.
4. If the count of `0`s and `1`s are equal, update the length of the maximum found balanced subarray.

### Code
```csharp
public int FindMaxLength(int[] nums) {
    int maxLength = 0;
    
    // Iterate over each starting index for the subarray
    for (int i = 0; i < nums.Length; i++) {
        int count0 = 0;
        int count1 = 0;
        
        // Iterate over each ending index for the subarray
        for (int j = i; j < nums.Length; j++) {
            // Count number of 0's and 1's
            if (nums[j] == 0) {
                count0++;
            } else {
                count1++;
            }
            
            // If the number of 0's and 1's are equal
            if (count0 == count1) {
                maxLength = Math.Max(maxLength, j - i + 1);
            }
        }
    }
    
    return maxLength;
}
```

### Complexity Analysis
- **Time Complexity**: O(n^2) - Two nested loops through the array.
- **Space Complexity**: O(1) - No additional data structures are used.

---

## Approach 2: HashMap Approach

### Intuition
The optimized approach uses a HashMap (dict) to store the difference between the count of `1`s and `0`s as a key and its associated index as the value. As we iterate through the array, we maintain this difference, and whenever the same difference appears again, it implies that the subarray between these indices is balanced.

### Steps
1. Initialize a HashMap to store the difference (count of 1s minus count of 0s).
2. Use a running sum to maintain the current difference.
3. If the running sum encounters the same value, compute the subarray length and update the maximum length.
4. Store the first occurrence of each running sum value in the HashMap.

### Code
```csharp
public int FindMaxLength(int[] nums) {
    Dictionary<int, int> prefixSumIndex = new Dictionary<int, int>();
    prefixSumIndex[0] = -1; // Base case: to handle the subarray from the start
    int maxLength = 0;
    int runningSum = 0;
    
    for (int i = 0; i < nums.Length; i++) {
        // Increase running sum for '1' and decrease for '0'
        runningSum += nums[i] == 1 ? 1 : -1;
        
        if (prefixSumIndex.ContainsKey(runningSum)) {
            // Update maxLength with the distance between the current index and the index where the same runningSum was seen
            maxLength = Math.Max(maxLength, i - prefixSumIndex[runningSum]);
        } else {
            // Store the index against this runningSum
            prefixSumIndex[runningSum] = i;
        }
    }
    
    return maxLength;
}
```

### Complexity Analysis
- **Time Complexity**: O(n) - We traverse the array once.
- **Space Complexity**: O(n) - Potentially storing each running sum in the HashMap.

---

