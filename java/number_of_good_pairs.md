# [1512. Number of Good Pairs](https://leetcode.com/problems/number-of-good-pairs/)

## Approach 1: Brute Force

### Solution
```java
// Time Complexity: O(n^2)
// Space Complexity: O(1)
public class Solution {
    public int numIdenticalPairs(int[] nums) {
        int count = 0;

        // Compare every pair of elements
        for (int i = 0; i < nums.length; i++) {
            for (int j = i + 1; j < nums.length; j++) {
                if (nums[i] == nums[j]) {
                    count++; // Increment count if pair is "good"
                }
            }
        }

        return count;
    }
}
```

## Approach 2: HashMap for Counting Frequencies

### Solution
```java
// Time Complexity: O(n)
// Space Complexity: O(n)
import java.util.HashMap;

public class Solution {
    public int numIdenticalPairs(int[] nums) {
        HashMap<Integer, Integer> countMap = new HashMap<>();
        int count = 0;

        // Count the occurrences of each number
        for (int num : nums) {
            if (countMap.containsKey(num)) {
                count += countMap.get(num); // Add the current count of this number
            }
            countMap.put(num, countMap.getOrDefault(num, 0) + 1); // Increment the count
        }

        return count;
    }
}
```

## Approach 3: Frequency Array (Optimized for Small Range)

### Solution
```java
// Time Complexity: O(n)
// Space Complexity: O(1) (assuming the range of numbers is limited)
public class Solution {
    public int numIdenticalPairs(int[] nums) {
        int[] freq = new int[101]; // Frequency array for numbers in range [1, 100]
        int count = 0;

        // Count pairs using frequency
        for (int num : nums) {
            count += freq[num]; // Add the number of pairs that can be formed
            freq[num]++; // Increment the frequency of the current number
        }

        return count;
    }
}
```

