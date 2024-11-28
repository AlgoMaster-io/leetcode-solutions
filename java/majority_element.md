# [169. Majority Element](https://leetcode.com/problems/majority-element/)

## Approach 1: Brute Force

### Solution
```java
// Time Complexity: O(n^2)
// Space Complexity: O(1)
public class Solution {
    public int majorityElement(int[] nums) {
        int majorityCount = nums.length / 2;

        // Check the count of each element
        for (int num : nums) {
            int count = 0;
            for (int element : nums) {
                if (element == num) {
                    count++;
                }
            }
            // If count exceeds majority, return the element
            if (count > majorityCount) {
                return num;
            }
        }

        return -1; // Shouldn't reach here as per problem constraints
    }
}
```

## Approach 2: HashMap to Count Frequencies

### Solution
```java
// Time Complexity: O(n)
// Space Complexity: O(n)
import java.util.HashMap;

public class Solution {
    public int majorityElement(int[] nums) {
        HashMap<Integer, Integer> countMap = new HashMap<>();
        int majorityCount = nums.length / 2;

        // Count frequencies of elements
        for (int num : nums) {
            countMap.put(num, countMap.getOrDefault(num, 0) + 1);

            // If an element's count exceeds majority, return it
            if (countMap.get(num) > majorityCount) {
                return num;
            }
        }

        return -1; // Shouldn't reach here as per problem constraints
    }
}
```

## Approach 3: Sorting

### Solution
```java
// Time Complexity: O(n log n)
// Space Complexity: O(1)
import java.util.Arrays;

public class Solution {
    public int majorityElement(int[] nums) {
        // Sort the array
        Arrays.sort(nums);

        // The majority element will always be at the middle index
        return nums[nums.length / 2];
    }
}
```

## Approach 4: Boyer-Moore Voting Algorithm

### Solution
```java
// Time Complexity: O(n)
// Space Complexity: O(1)
public class Solution {
    public int majorityElement(int[] nums) {
        int count = 0;
        Integer candidate = null;

        // Find the candidate for the majority element
        for (int num : nums) {
            if (count == 0) {
                candidate = num; // Set a new candidate
            }
            count += (num == candidate) ? 1 : -1;
        }

        return candidate; // The problem guarantees the majority element exists
    }
}
```

