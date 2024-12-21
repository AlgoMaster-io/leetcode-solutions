# [Leetcode 260: Single Number III](https://leetcode.com/problems/single-number-iii/)

## Approaches:

1. [Using HashMap](#using-hashmap)
2. [Using Bit Manipulation](#using-bit-manipulation)

---

### Using HashMap

#### Approach:
The simplest approach to solve this problem is to use a HashMap to count the occurrences of each number in the array. We then find the two numbers that appear exactly once by iterating through the map.

#### Intuition:
This approach utilizes extra space to store each number's count, but guarantees to find the solution using a single pass over the map.

#### Code:
```java
import java.util.HashMap;
import java.util.Map;

public class Solution {
    public int[] singleNumber(int[] nums) {
        // Create a hashmap to store the count of each number
        Map<Integer, Integer> countMap = new HashMap<>();
        
        // Iterate through the array, updating the count of each number
        for (int num : nums) {
            countMap.put(num, countMap.getOrDefault(num, 0) + 1);
        }
        
        // Prepare the result array for the two unique numbers
        int[] result = new int[2];
        int index = 0;
        
        // Iterate through the map and find the numbers with a count of 1
        for (Map.Entry<Integer, Integer> entry : countMap.entrySet()) {
            if (entry.getValue() == 1) {
                result[index++] = entry.getKey();
                
                // Stop after finding the two unique numbers
                if (index == 2) {
                    break;
                }
            }
        }
        
        return result;
    }
}
```

#### Time Complexity:
- O(n): We iterate through the array and then through the map.

#### Space Complexity:
- O(n): We use additional space for the HashMap.

---

### Using Bit Manipulation

#### Approach:
A more optimal solution uses bit manipulation. First, XOR all numbers to find the XOR of the two unique numbers. Then, find a bit that is set (1) in the XOR result and use it to differentiate the two unique numbers.

#### Intuition:
- XOR operation between two identical numbers will cancel each other out to 0.
- By XORing all numbers, every pair of identical numbers cancels out, leaving us with the XOR of the two unique numbers.
- A single set bit in the XOR result tells us which bit differentiates the two unique numbers.
- We can use this bit to separate numbers into two groups and find the two unique numbers by XORing within these groups.

#### Code:
```java
public class Solution {
    public int[] singleNumber(int[] nums) {
        // Step 1: XOR all numbers to get the result of two unique numbers XORed
        int xor = 0;
        for (int num : nums) {
            xor ^= num;
        }
        
        // Step 2: Find any bit that is set in the xor result (we'll use the rightmost bit)
        int diff = xor & (-xor); // This finds the rightmost set bit
        
        // Step 3: Divide all numbers into two groups based on the found bit
        int[] result = new int[2];
        for (int num : nums) {
            if ((num & diff) == 0) {
                result[0] ^= num; // Group 1: with the bit not set
            } else {
                result[1] ^= num; // Group 2: with the bit set
            }
        }
        
        return result;
    }
}
```

#### Time Complexity:
- O(n): We make a single pass to XOR all elements and another pass to separate them into groups and find results.

#### Space Complexity:
- O(1): We use a constant amount of extra space beyond the input and output.

---

