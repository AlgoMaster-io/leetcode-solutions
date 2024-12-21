# [Leetcode 1512: Number of Good Pairs](https://leetcode.com/problems/number-of-good-pairs/)

## Solutions:

- [Approach 1: Brute Force](#approach-1)
- [Approach 2: Optimized using HashMap](#approach-2)

### Approach 1: Brute Force

**Intuition:**

The simplest approach is to check every possible pair `(i, j)` where `0 <= i < j < nums.length` and check if they are equal. If they are, we have found a "good pair."

**Detailed Steps:**

1. Initialize a counter `count` to zero to keep track of good pairs.
2. Use two nested loops, the outer loop (`i`) goes from `0` to `nums.length - 1`, and the inner loop (`j`) runs from `i+1` to `nums.length - 1`.
3. For each pair `(i, j)`, check if `nums[i] == nums[j]`. If yes, increment the `count`.
4. Return the `count` after both loops have finished.

**Time Complexity:** O(n^2)  
**Space Complexity:** O(1)

```java
public class Solution {
    public int numIdenticalPairs(int[] nums) {
        int count = 0;
        // Outer loop to fix the first element of the pair
        for (int i = 0; i < nums.length; i++) {
            // Inner loop to fix the second element of the pair
            for (int j = i + 1; j < nums.length; j++) {
                // Check if we have a good pair
                if (nums[i] == nums[j]) {
                    count++;
                }
            }
        }
        return count;
    }
}
```

### Approach 2: Optimized using HashMap

**Intuition:**

Instead of comparing each pair, count the occurrences of each number using a HashMap. For each duplicate occurrence of a number, calculate the number of good pairs using the formula for combinations: 
\[ \text{Pairs} = \frac{n(n-1)}{2} \]

Where `n` is the count of occurrences of a number.

**Detailed Steps:**

1. Use a HashMap to store the frequency of each number in the array.
2. Iterate through the array, updating the frequency of each number in the HashMap.
3. For each number encountered again, increment the count of good pairs using the formula for combinations since each new occurrence can pair with all previous ones.
4. Return the total count of good pairs.

**Time Complexity:** O(n)  
**Space Complexity:** O(n)

```java
import java.util.HashMap;

public class Solution {
    public int numIdenticalPairs(int[] nums) {
        HashMap<Integer, Integer> map = new HashMap<>();
        int count = 0;
        
        // Iterate over each number in the array
        for (int num : nums) {
            // If number is already seen, it can form pairs with all previous occurrences
            if (map.containsKey(num)) {
                // Increment count by the number of occurrences seen so far
                count += map.get(num);
                // Increment the occurrence count in the map
                map.put(num, map.get(num) + 1);
            } else {
                // If number is seen for the first time, put it in the map
                map.put(num, 1);
            }
        }
        return count;
    }
}
```

These solutions cover both the brute force method for conceptual understanding and an optimized approach for practical use, providing an effective balance between clarity and efficiency.

