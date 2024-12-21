# [Leetcode 424: Longest Repeating Character Replacement](https://leetcode.com/problems/longest-repeating-character-replacement/)

## Approaches
- [Sliding Window with HashMap](#sliding-window-with-hashmap)
- [Optimized Sliding Window](#optimized-sliding-window)

### Sliding Window with HashMap

**Intuition:**
The problem requires us to determine the length of the longest substring that can be made up of the same character by replacing at most 'k' characters. A naive approach would evaluate each substring, but that would be inefficient. Instead, we utilize the sliding window technique for efficiency.

A dynamic sliding window is ideal here as it allows us to explore parts of the string. We'll adjust the window's size and location on the string to check potential substrings.

We use a HashMap to keep track of the character frequency in the current window. The main trick is to maintain a window where it is possible to replace at most 'k' characters. If a window has more than 'k' characters that need replacing, it shrinks from the left.

**Steps:**
1. Initiate two pointers, left and right, and a HashMap for character frequency count.
2. Expand the window by moving the right pointer.
3. Calculate the maximum frequency of any character in the current window.
4. If the (window size - max frequency character) exceeds 'k', it means we have to shift the left pointer rightward to potentially create a valid window.
5. Continue until the right pointer reaches the end of the string.

```java
public int characterReplacement(String s, int k) {
    int left = 0, maxCount = 0, maxLength = 0;
    int[] count = new int[26];
    
    for (int right = 0; right < s.length(); right++) {
        // Increment character count for the character at 'right'
        maxCount = Math.max(maxCount, ++count[s.charAt(right) - 'A']);
        
        // Window size: right - left + 1
        // Calculate if replacements exceed k
        if (right - left + 1 - maxCount > k) {
            count[s.charAt(left) - 'A']--;
            left++;
        }
        
        // Update maximum length of the window
        maxLength = Math.max(maxLength, right - left + 1);
    }
    
    return maxLength;
}
```

**Time Complexity:** O(N) - where N is the length of the string. We potentially examine each character once as we adjust our window.
**Space Complexity:** O(1) - Our frequency map is constant size.

### Optimized Sliding Window

**Intuition:**
The above sliding window approach is fairly optimal, but it can be slightly tweaked for better clarity. The main observation is that instead of recalculating the maximum frequency character during every iteration, we can leverage the maximum count so far since any extension of the window will naturally satisfy conditions or prompt a shrinking of the window.

**Steps:**
1. As in the previous approach, use two pointers for the window.
2. Increment the frequency of the right character and update the maximum frequency.
3. Adjust the window size to always satisfy the constraint of at most 'k' replacements.
4. Continuously update the maxLength of the window.

```java
public int characterReplacement(String s, int k) {
    int[] count = new int[26];
    int maxCount = 0, maxLength = 0;
    int left = 0;
    
    for (int right = 0; right < s.length(); right++) {
        maxCount = Math.max(maxCount, ++count[s.charAt(right) - 'A']);
        
        // If we need to replace more than k characters, move left
        while (right - left + 1 - maxCount > k) {
            count[s.charAt(left) - 'A']--;
            left++;
        }
        
        maxLength = Math.max(maxLength, right - left + 1);
    }
    
    return maxLength;
}
```

**Time Complexity:** O(N) 
**Space Complexity:** O(1)

Both approaches are utilizing the sliding window concept, where we dynamically adjust our window to fit constraints, making sure to potentially explore every character of the string only once. This is optimal for the problem at hand with a linear time complexity relative to the input size.

