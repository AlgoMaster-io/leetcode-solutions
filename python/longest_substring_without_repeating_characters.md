# [3. Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters/)

## Approach 1: Brute Force (Basic)

### Solution
```python
# Time Complexity: O(n^2)
# Space Complexity: O(n)

class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        max_length = 0

        # Check every possible substring
        for i in range(len(s)):
            seen = set()
            length = 0

            for j in range(i, len(s)):
                if s[j] in seen:
                    break  # Break if a duplicate character is found
                seen.add(s[j])
                length += 1
                max_length = max(max_length, length)  # Update max_length

        return max_length
```

## Approach 2: Sliding Window with HashSet

### Solution
```python
# Time Complexity: O(n)
# Space Complexity: O(n)

class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        set_chars = set()
        max_length = 0
        left = 0

        # Sliding window
        for right in range(len(s)):
            while s[right] in set_chars:
                set_chars.remove(s[left])  # Shrink the window
                left += 1
            set_chars.add(s[right])  # Expand the window
            max_length = max(max_length, right - left + 1)  # Update max_length

        return max_length
```

## Approach 3: Sliding Window with HashMap (Optimal)

### Solution
```python
# Time Complexity: O(n)
# Space Complexity: O(n)

class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        char_index_map = {}
        max_length = 0
        left = 0

        # Sliding window
        for right in range(len(s)):
            current_char = s[right]

            # If the character is already in the map, update the left pointer
            if current_char in char_index_map:
                left = max(left, char_index_map[current_char] + 1)

            # Update the character's last index in the map
            char_index_map[current_char] = right

            # Update max_length
            max_length = max(max_length, right - left + 1)

        return max_length
```

