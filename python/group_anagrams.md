# [49. Group Anagrams](https://leetcode.com/problems/group-anagrams/)

## Approach 1: Sorting + HashMap

### Solution
python
```python
# Time Complexity: O(n * k * log k) where n is the number of strings and k is the average string length
# Space Complexity: O(n * k)
from collections import defaultdict
from typing import List

class Solution:
    def groupAnagrams(self, strs: List[str]) -> List[List[str]]:
        # Map to group anagrams by their sorted representation
        anagrams = defaultdict(list)

        for s in strs:
            # Sort the string to create a key for anagrams
            sorted_str = ''.join(sorted(s))

            # Add the string to the corresponding group
            anagrams[sorted_str].append(s)

        # Return all groups of anagrams
        return list(anagrams.values())
```

## Approach 2: Frequency Count + HashMap

### Solution
python
```python
# Time Complexity: O(n * k) where n is the number of strings and k is the average string length
# Space Complexity: O(n * k)
from collections import defaultdict
from typing import List

class Solution:
    def groupAnagrams(self, strs: List[str]) -> List[List[str]]:
        # Map to group anagrams by their frequency count representation
        anagrams = defaultdict(list)

        for s in strs:
            # Create a frequency count array for the string
            count = [0] * 26
            for char in s:
                count[ord(char) - ord('a')] += 1

            # Convert frequency array to a string key
            key = '#'.join(map(str, count))

            # Add the string to the corresponding group
            anagrams[key].append(s)

        # Return all groups of anagrams
        return list(anagrams.values())
```

## Approach 3: Prime Product Hashing (Alternative)

### Solution
python
```python
# Time Complexity: O(n * k) where n is the number of strings and k is the average string length
# Space Complexity: O(n * k)
from collections import defaultdict
from typing import List

class Solution:
    def groupAnagrams(self, strs: List[str]) -> List[List[str]]:
        # Assign a unique prime number to each character
        primes = [2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37, 41, 43, 47, 
                  53, 59, 61, 67, 71, 73, 79, 83, 89, 97, 101]
        anagrams = defaultdict(list)

        for s in strs:
            product = 1
            for char in s:
                product *= primes[ord(char) - ord('a')]  # Compute unique hash using prime products

            # Add the string to the corresponding group
            anagrams[product].append(s)

        # Return all groups of anagrams
        return list(anagrams.values())
```

