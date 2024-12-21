# [205. Isomorphic Strings](https://leetcode.com/problems/isomorphic-strings/)

## Approaches

- [Approach 1: Brute Force Mapping](#approach-1-brute-force-mapping)
- [Approach 2: One-to-One Mapping with Two Hash Maps](#approach-2-one-to-one-mapping-with-two-hash-maps)
- [Approach 3: Single Hash Map with Character Encoding](#approach-3-single-hash-map-with-character-encoding)

---

## Approach 1: Brute Force Mapping

### Intuition
The idea behind this approach is to manually map each character from the first string `s` to the second string `t` and vice versa. We iterate over the strings, and for each character, we verify whether it has previously been mapped to a different character.

### Steps:
1. Traverse each character of both strings at the same time.
2. Maintain two dictionaries: one for mapping characters in `s` to characters in `t`, and another for mapping characters in `t` to characters in `s`.
3. For each pair of characters from `s` and `t`, check:
   - If the character from `s` is already mapped to another character in `t`.
   - If the character from `t` is already mapped to another character in `s`.
4. If any of the above checks fail, return `False`.
5. If we successfully traverse both strings without conflict, return `True`.

### Code
```python
def is_isomorphic(s: str, t: str) -> bool:
    if len(s) != len(t):
        return False

    map_s_to_t = {}
    map_t_to_s = {}
    
    for c1, c2 in zip(s, t):
        # Check if mappings are not consistent
        if (c1 in map_s_to_t and map_s_to_t[c1] != c2) or (c2 in map_t_to_s and map_t_to_s[c2] != c1):
            return False
        
        # Create mappings
        map_s_to_t[c1] = c2
        map_t_to_s[c2] = c1
        
    return True
```

### Complexity
- **Time Complexity:** \(O(n)\), where \(n\) is the length of the strings. We go through the strings once to create and check mappings.
- **Space Complexity:** \(O(n)\) in the worst case for storing mappings in hash maps.

---

## Approach 2: One-to-One Mapping with Two Hash Maps

### Intuition
This approach optimizes the brute force solution by combining the mapping into one function while still using two maps to keep track of the mapping relations. The idea remains the same, but is implemented more efficiently.

### Steps:
1. Initialize two dictionaries for forward and reverse mapping.
2. Traverse characters of `s` and `t` simultaneously.
3. Use dictionaries to verify if existing mappings are consistent.
4. Return `False` if a conflict in mappings is detected.
5. If the traversal completes without conflict, return `True`.

### Code
```python
def is_isomorphic(s: str, t: str) -> bool:
    if len(s) != len(t):
        return False

    map_s_to_t, map_t_to_s = {}, {}
    
    for c1, c2 in zip(s, t):
        if c1 in map_s_to_t:
            if map_s_to_t[c1] != c2:
                return False
        else:
            map_s_to_t[c1] = c2
        
        if c2 in map_t_to_s:
            if map_t_to_s[c2] != c1:
                return False
        else:
            map_t_to_s[c2] = c1
    
    return True
```

### Complexity
- **Time Complexity:** \(O(n)\), where \(n\) is the length of the strings.
- **Space Complexity:** \(O(n)\), due to the space used by the two dictionaries for mappings.

---

## Approach 3: Single Hash Map with Character Encoding

### Intuition
This optimal approach is based on encoding the characters' first occurrence in their respective strings and comparing these encoded representations. The idea is to use one hash map to assign unique identifiers (or indices) to characters based on their order of appearance.

### Steps:
1. Traverse both strings concurrently.
2. Keep two lists or strings, appending indices that signify the first occurrence of the characters.
3. Compare these serializations.
4. If they are identical, the strings are isomorphic.

### Code
```python
def is_isomorphic(s: str, t: str) -> bool:
    def encode(string):
        mapping = {}
        encoded_string = []
        for index, char in enumerate(string):
            if char not in mapping:
                mapping[char] = index
            encoded_string.append(mapping[char])
        return encoded_string
    
    return encode(s) == encode(t)
```

### Complexity
- **Time Complexity:** \(O(n)\), with \(n\) representing the length of the strings.
- **Space Complexity:** \(O(n)\), for the additional space used by the encoding lists.

