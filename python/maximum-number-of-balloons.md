### [Leetcode Problem 1189: Maximum Number of Balloons](https://leetcode.com/problems/maximum-number-of-balloons/)

**Approach Navigation:**
1. [Basic Approach: Frequency Count](#basic-approach-frequency-count)
2. [Optimal Approach: Using Counter from Collections](#optimal-approach-using-counter-from-collections)

---

## Basic Approach: Frequency Count

### Intuition:
The task is to determine how many times the word "balloon" can be formed using the letters in a given string `text`. The word "balloon" consists of `b, a, l, l, o, o, n`. We need to track the frequency of these letters and ensure we have enough to form the word in entirety.

### Steps:
1. Create a frequency dictionary for the letters in `text`.
2. Check the minimum number of times we can form the word "balloon" by considering the frequency of each letter needed:
   - 'b': needs 1 instance
   - 'a': needs 1 instance
   - 'l': needs 2 instances
   - 'o': needs 2 instances
   - 'n': needs 1 instance
3. Calculate the count for each required letter by evaluating how many full instances can be formed based on the existing frequency.
4. Return the minimum count across all required letter calculations.

### Code:

```python
def maxNumberOfBalloons(text: str) -> int:
    # Frequency dictionary for the text
    freq = {}
    for char in text:
        if char in freq:
            freq[char] += 1
        else:
            freq[char] = 1
    
    # Calculate the maximum instances of "balloon"
    min_instances = float('inf')
    min_instances = min(min_instances, freq.get('b', 0))
    min_instances = min(min_instances, freq.get('a', 0))
    min_instances = min(min_instances, freq.get('l', 0) // 2)
    min_instances = min(min_instances, freq.get('o', 0) // 2)
    min_instances = min(min_instances, freq.get('n', 0))
    
    return min_instances

# Time Complexity: O(n), where n is the length of the text. We visit each character once.
# Space Complexity: O(1), since we are using a fixed size dictionary for a constant set of characters.
```

---

## Optimal Approach: Using Counter from Collections

### Intuition:
Utilize Python's `collections.Counter` to simplify the counting process. This module makes it easy to create a frequency dictionary and simplifies our logic for determining the minimum instances of "balloon" that can be formed.

### Steps:
1. Use `Counter` to get the frequency of each character in `text`.
2. Retrieve the counts for each character relevant to "balloon".
3. Compute how many full instances of "balloon" can be formed based on these frequencies.

### Code:

```python
from collections import Counter

def maxNumberOfBalloons(text: str) -> int:
    # Create a Counter for all characters in text
    freq = Counter(text)
    
    # Calculate instances for each letter needed in 'balloon'
    b_count = freq['b']
    a_count = freq['a']
    l_count = freq['l'] // 2  # Need 2 'l's
    o_count = freq['o'] // 2  # Need 2 'o's
    n_count = freq['n']
    
    # Find the minimum of these counts
    return min(b_count, a_count, l_count, o_count, n_count)

# Time Complexity: O(n), where n is the length of the text. The Counter operations run in linear time.
# Space Complexity: O(1), using a fixed dict for characters in "balloon".
```

Both approaches efficiently compute the solution, but using `collections.Counter` makes the code cleaner and leverages Python's built-in methods for frequency counting.

