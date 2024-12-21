## [Problem: Leetcode 383 - Ransom Note](https://leetcode.com/problems/ransom-note/)

### Approaches
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: HashMap / Count Array](#approach-2-hashmap--count-array)
- [Approach 3: Counter from collections](#approach-3-counter-from-collections)

---

### Approach 1: Brute Force

#### Intuition
The simplest way to solve this problem is to iterate over each character in the ransom note and check if it exists in the magazine. After finding a character in the magazine, we remove it so it can't be used again.

#### Steps
1. Convert the magazine string to a list to make character removal easier.
2. For each character in the ransom note, check if it is in the list.
3. If found, remove it from the list.
4. If any character in the ransom note is not found, return False.
5. If all characters are found and removed, return True.

```python
def canConstruct(ransomNote: str, magazine: str) -> bool:
    magazine_list = list(magazine)
    # Iterate through each character in ransom note
    for char in ransomNote:
        if char in magazine_list:
            # Remove the first occurrence of the character from magazine list
            magazine_list.remove(char)
        else:
            # If the character is not found in magazine list
            return False
    return True
```

**Time Complexity:** O(n^2) - For each character in the ransom note, we might have to traverse the magazine list to find and remove the character.

**Space Complexity:** O(n) - We are using a list to store the characters of the magazine.

---

### Approach 2: HashMap / Count Array

#### Intuition
By counting the number of occurrences of each character in both the ransom note and the magazine, we can efficiently determine if the magazine contains enough of each character to construct the ransom note.

#### Steps
1. Create a count dictionary or array for the magazine to store the frequency of each character.
2. Iterate over each character in the ransom note.
3. Check if the magazine count is enough for each character in the ransom note.
4. If any character is insufficient, return False.
5. If all characters in the ransom note have sufficient counts in the magazine, return True.

```python
def canConstruct(ransomNote: str, magazine: str) -> bool:
    from collections import defaultdict
    
    char_count = defaultdict(int)
    
    # Count frequency of each character in the magazine
    for char in magazine:
        char_count[char] += 1
    
    # Check if we have enough characters for the ransom note
    for char in ransomNote:
        if char_count[char] <= 0:
            return False
        char_count[char] -= 1
    
    return True
```

**Time Complexity:** O(n + m) - n is the length of the magazine and m is the length of the ransom note.

**Space Complexity:** O(n) - We store the frequency of magazine characters in a dictionary.

---

### Approach 3: Counter from Collections

#### Intuition
Using Python's `collections.Counter`, this approach becomes very succinct. By leveraging an existing library function, we can avoid a lot of boilerplate code.

#### Steps
1. Create Counter objects for both the magazine and the ransom note.
2. Initialize a loop through the ransom note's character counts.
3. Check if the magazine's count for each character is at least as large as the ransom note's count.
4. If all checks pass, return True, otherwise return False.

```python
from collections import Counter

def canConstruct(ransomNote: str, magazine: str) -> bool:
    ransom_counter = Counter(ransomNote)
    magazine_counter = Counter(magazine)
    
    # Check if magazine has enough of each character
    for char, count in ransom_counter.items():
        if magazine_counter[char] < count:
            return False
    return True
```

**Time Complexity:** O(n + m) - Where n is the length of the magazine and m is the length of the ransom note.

**Space Complexity:** O(1) - The space complexity is technically O(n) for storing the character counts but generally considered O(1) since we're limited by a constant character set.

