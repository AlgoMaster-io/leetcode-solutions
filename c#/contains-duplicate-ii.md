# [Contains Duplicate II](https://leetcode.com/problems/contains-duplicate-ii/)

## Approaches
- [Brute Force Approach](#brute-force-approach)
- [HashMap Approach](#hashmap-approach)

### Brute Force Approach

#### Intuition
The most straightforward approach to this problem is to use two nested loops to compare each element with every other element within the range `k`. If we find any such pair `(i, j)` such that `nums[i] == nums[j]` and the absolute difference `|i - j| <= k`, then return `true`. Otherwise, return `false`.

#### Solution
```csharp
public class Solution {
    public bool ContainsNearbyDuplicate(int[] nums, int k) {
        // Iterate over each element
        for (int i = 0; i < nums.Length; i++) {
            // Compare current element with elements in the window of size k
            for (int j = i + 1; j <= i + k && j < nums.Length; j++) {
                if (nums[i] == nums[j]) {
                    return true; // Return true if duplicate found within k distance
                }
            }
        }
        return false; // Return false if no such pair is found
    }
}
```

#### Complexity Analysis
- **Time Complexity**: \(O(n \cdot k)\), where \(n\) is the number of elements in the array. 
- **Space Complexity**: \(O(1)\), as no additional space is required apart from variables.

### HashMap Approach

#### Intuition
We can optimize the brute force solution by using a HashMap (or Dictionary in C#) to store each number's last index as we traverse the array. Whenever we encounter a number that already exists in the HashMap, we check if the difference of indices is \(\leq k\). This approach reduces the time complexity significantly as we only need to pass through the array once.

#### Solution
```csharp
public class Solution {
    public bool ContainsNearbyDuplicate(int[] nums, int k) {
        Dictionary<int, int> map = new Dictionary<int, int>();
        // Traverse the array
        for (int i = 0; i < nums.Length; i++) {
            if (map.ContainsKey(nums[i])) {
                // Check if the current number was seen within the last k indices
                if (i - map[nums[i]] <= k) {
                    return true;
                }
            }
            // Update the last seen index of the current number
            map[nums[i]] = i;
        }
        return false; // Return false if no such index pair is found
    }
}
```

#### Complexity Analysis
- **Time Complexity**: \(O(n)\), as we pass through the array once and access the dictionary in \(O(1)\) on average.
- **Space Complexity**: \(O(n)\), due to the additional storage required for the dictionary to store indices.

