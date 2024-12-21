# [Leetcode 560: Subarray Sum Equals K](https://leetcode.com/problems/subarray-sum-equals-k/)

## Approaches
1. [Brute Force Approach](#approach-1-brute-force-approach)
2. [Optimized Approach Using HashMap (Prefix Sum)](#approach-2-optimized-approach-using-hashmap-prefix-sum)

---

## Approach 1: Brute Force Approach

### Intuition
The brute force approach involves checking every possible subarray within a given array to determine if the sum of its elements equals `k`. We use a nested loop where the outer loop iterates over the starting index of the subarray and the inner loop calculates the sum of the elements from this starting index. If the sum equals `k`, we increment our count.

### Time Complexity
- **Time:** \(O(n^2)\), where \(n\) is the length of the array. This is because for each starting index, we potentially iterate over the remaining elements to check for possible subarrays.
- **Space:** \(O(1)\), no additional space apart from variables.

### Solution Code
```typescript
function subarraySum(nums: number[], k: number): number {
    let count = 0;
    
    for (let start = 0; start < nums.length; start++) {
        let sum = 0;
        for (let end = start; end < nums.length; end++) {
            sum += nums[end]; // Calculate sum of subarray from start to end
            if (sum === k) { // Check if current sum equals k
                count++; // Increment count if sum is k
            }
        }
    }
    
    return count; // Return total count of subarrays
}
```

---

## Approach 2: Optimized Approach Using HashMap (Prefix Sum)

### Intuition
To avoid recalculating the sum of every subarray repeatedly, we can use a prefix sum approach with a HashMap. The idea is to keep track of cumulative sums (or prefix sums) at each index and store their frequencies in the HashMap. By checking the difference between the current prefix sum and `k`, we can determine how many subarrays sum up to `k` ending at the current index.

### Detailed Steps:
1. Maintain a running sum of the array as you iterate through it.
2. For each element, the current cumulative sum is calculated.
3. If this cumulative sum minus `k` has been seen before in the HashMap, it means there are some subarrays that sum to `k`.
4. Add the frequency of this sum difference to the count of subarrays.
5. Update the HashMap with the current cumulative sum.

### Time Complexity
- **Time:** \(O(n)\), where \(n\) is the length of the array, as we make a single pass through the array.
- **Space:** \(O(n)\), due to storing the prefix sums in the HashMap.

### Solution Code
```typescript
function subarraySum(nums: number[], k: number): number {
    let count = 0;
    let sum = 0;
    const prefixSumFrequency = new Map<number, number>();
    prefixSumFrequency.set(0, 1); // Base case for a subarray that starts from the beginning
    
    for (let num of nums) {
        sum += num; // Update the running sum
        
        // If (sum - k) exists in our map, it means there are some subarrays which sum up to 'k'
        if (prefixSumFrequency.has(sum - k)) {
            count += prefixSumFrequency.get(sum - k)!; // Increment count by the frequency of the needed sum
        }
        
        // Update the frequency of the current sum in the map
        prefixSumFrequency.set(sum, (prefixSumFrequency.get(sum) || 0) + 1);
    }
    
    return count; // Return the count of subarrays summing up to k
}
```

Each approach presented progressively enhances efficiency, culminating in a highly optimized solution using prefix sums and HashMaps to achieve linear time complexity.

