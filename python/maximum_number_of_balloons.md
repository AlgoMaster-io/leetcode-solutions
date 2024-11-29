# [1189. Maximum Number of Balloons](https://leetcode.com/problems/maximum-number-of-balloons/)

## Approach 1: Frequency Count with Dictionary

### Solution
```python
# Time Complexity: O(n)
# Space Complexity: O(1)
from collections import defaultdict

class Solution:
    def maxNumberOfBalloons(self, text: str) -> int:
        count_map = defaultdict(int)

        # Count frequencies of each character in the text
        for c in text:
            count_map[c] += 1

        # Check the minimum occurrences of characters required for "balloon"
        b = count_map.get('b', 0)
        a = count_map.get('a', 0)
        l = count_map.get('l', 0) // 2  # 'l' appears twice in "balloon"
        o = count_map.get('o', 0) // 2  # 'o' appears twice in "balloon"
        n = count_map.get('n', 0)

        # Return the minimum number of complete "balloon" words
        return min(b, a, l, o, n)
```

## Approach 2: Frequency Count with List

### Solution
```python
# Time Complexity: O(n)
# Space Complexity: O(1)
class Solution:
    def maxNumberOfBalloons(self, text: str) -> int:
        char_count = [0] * 26  # Frequency array for all characters

        # Count frequencies of each character in the text
        for c in text:
            char_count[ord(c) - ord('a')] += 1

        # Check the minimum occurrences of characters required for "balloon"
        b = char_count[ord('b') - ord('a')]
        a = char_count[ord('a') - ord('a')]
        l = char_count[ord('l') - ord('a')] // 2  # 'l' appears twice in "balloon"
        o = char_count[ord('o') - ord('a')] // 2  # 'o' appears twice in "balloon"
        n = char_count[ord('n') - ord('a')]

        # Return the minimum number of complete "balloon" words
        return min(b, a, l, o, n)
```

## Approach 3: Optimized Single Pass

### Solution
```python
# Time Complexity: O(n)
# Space Complexity: O(1)
class Solution:
    def maxNumberOfBalloons(self, text: str) -> int:
        counts = [0] * 5  # Indices: 0->b, 1->a, 2->l, 3->o, 4->n

        # Count only relevant characters
        for c in text:
            if c == 'b':
                counts[0] += 1
            elif c == 'a':
                counts[1] += 1
            elif c == 'l':
                counts[2] += 1
            elif c == 'o':
                counts[3] += 1
            elif c == 'n':
                counts[4] += 1

        # 'l' and 'o' need to be divided by 2 as they appear twice in "balloon"
        counts[2] //= 2
        counts[3] //= 2

        # Return the minimum count
        return min(counts)
```

