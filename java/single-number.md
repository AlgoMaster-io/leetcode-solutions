# [136. Single Number](https://leetcode.com/problems/single-number/)

## Approaches:
- [Brute Force Approach](#brute-force-approach)
- [HashMap Approach](#hashmap-approach)
- [Bit Manipulation Approach](#bit-manipulation-approach)

---

### Brute Force Approach

**Intuition:**  
In a brute force approach, for each element in the array, we can check if it appears again in the array. We can do this by comparing each element against all others. The element that doesn't have a duplicate is the single number.

```java
public int singleNumber(int[] nums) {
    // Iterate through each element in the array
    for (int i = 0; i < nums.length; i++) {
        boolean isSingle = true;
        // Compare the current element against all other elements
        for (int j = 0; j < nums.length; j++) {
            if (i != j && nums[i] == nums[j]) {
                isSingle = false; // If a duplicate is found, set the flag to false
                break;
            }
        }
        // If no duplicate is found, this is the single number
        if (isSingle) {
            return nums[i];
        }
    }
    return -1; // Edge case: should not reach here as the problem specifies one single element
}
```
**Time Complexity:** O(n^2)  
**Space Complexity:** O(1)

---

### HashMap Approach

**Intuition:**  
Utilize a HashMap to count the occurrences of each element. Then, iterate over the HashMap to find the element with a count of one, which is our single number.

```java
import java.util.HashMap;
import java.util.Map;

public int singleNumber(int[] nums) {
    // Initialize a HashMap to count occurrences of each element
    Map<Integer, Integer> countMap = new HashMap<>();
    
    // Populate the HashMap with the number of occurrences of each element
    for (int num : nums) {
        countMap.put(num, countMap.getOrDefault(num, 0) + 1);
    }
    
    // Iterate over the HashMap to find the single number
    for (Map.Entry<Integer, Integer> entry : countMap.entrySet()) {
        if (entry.getValue() == 1) {
            return entry.getKey(); // Found the single number
        }
    }
    
    return -1; // Should never reach here as problem guarantees a single element
}
```
**Time Complexity:** O(n)  
**Space Complexity:** O(n)

---

### Bit Manipulation Approach

**Intuition:**  
The most optimal solution involves using the XOR operation. XOR of a number with itself results in 0, and XOR with 0 is the number itself. Thus, XOR'ing all elements results in the single number because pairs cancel each other out.

```java
public int singleNumber(int[] nums) {
    int result = 0; // Initialize result which will hold our single number
    
    // XOR all elements; duplicates will cancel out
    for (int num : nums) {
        result ^= num; // XORing each number with the result
    }
    
    return result; // The resulting XOR is the single number
}
```
**Time Complexity:** O(n)  
**Space Complexity:** O(1)

---

