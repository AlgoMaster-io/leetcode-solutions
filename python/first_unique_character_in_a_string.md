# 387. [First Unique Character in a String](https://leetcode.com/problems/first-unique-character-in-a-string/)

## Approach 1: HashMap for Character Frequency

### Solution
```python
# Time Complexity: O(n)
# Space Complexity: O(n)

def firstUniqChar(s: str) -> int:
    count_map = {}
    
    # Count the frequency of each character
    for c in s:
        count_map[c] = count_map.get(c, 0) + 1

    # Find the first character with frequency 1
    for i in range(len(s)):
        if count_map[s[i]] == 1:
            return i

    return -1  # No unique character found
```

## Approach 2: Array for Character Frequency

### Solution
```python
# Time Complexity: O(n)
# Space Complexity: O(1)

def firstUniqChar(s: str) -> int:
    char_count = [0] * 26  # Array to store frequency of each character

    # Count the frequency of each character
    for c in s:
        char_count[ord(c) - ord('a')] += 1

    # Find the first character with frequency 1
    for i in range(len(s)):
        if char_count[ord(s[i]) - ord('a')] == 1:
            return i

    return -1  # No unique character found
```

## Approach 3: Single Pass with Index Array

### Solution
```python
# Time Complexity: O(n)
# Space Complexity: O(1)

def firstUniqChar(s: str) -> int:
    index = [-1] * 26  # Array to track occurrences and order of characters

    # Traverse string to fill occurrences and order
    for i in range(len(s)):
        char_index = ord(s[i]) - ord('a')
        if index[char_index] == -1:
            index[char_index] = i  # First occurrence
        else:
            index[char_index] = -2  # Mark as repeated

    # Find the smallest index of a unique character
    result = float('inf')
    for i in range(26):
        if index[i] >= 0:
            result = min(result, index[i])

    return -1 if result == float('inf') else result
```

