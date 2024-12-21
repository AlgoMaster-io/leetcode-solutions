# [205. Isomorphic Strings](https://leetcode.com/problems/isomorphic-strings/)

## Table of Contents
1. [Approach 1: Brute Force with Nested Loops](#approach-1)
2. [Approach 2: Hash Map for Mapping Characters](#approach-2)
3. [Approach 3: Hash Map for Mapping + Inverse Map Check](#approach-3)

---

## Approach 1: Brute Force with Nested Loops

### Intuition
The simplest approach is to compare the characters of the strings and ensure that the mapping between characters of the strings is consistent throughout the length. We need to check whether each character in the first string consistently maps to the same character in the second string.

### Implementation

```javascript
var isIsomorphic = function(s, t) {
    for (let i = 0; i < s.length; i++) {
        for (let j = i + 1; j < s.length; j++) {
            // Check if the current mapping is consistent
            if ((s[i] === s[j]) !== (t[i] === t[j])) {
                return false;
            }
        }
    }
    return true;
};
```

### Complexity Analysis
- **Time Complexity**: O(n^2), where n is the length of the string. The nested loop results in a quadratic time complexity.
- **Space Complexity**: O(1), no extra space used apart from variables.

---

## Approach 2: Hash Map for Mapping Characters

### Intuition
We use two hash maps to keep track of character mappings from `s` to `t` and vice versa. This mapping helps verify if both strings can be converted isomorphically by checking each character only once.

### Implementation

```javascript
var isIsomorphic = function(s, t) {
    if (s.length !== t.length) return false;
    
    let mapST = new Map(); // map for s -> t
    let mapTS = new Map(); // map for t -> s
    
    for (let i = 0; i < s.length; i++) {
        let charS = s[i];
        let charT = t[i];
        
        // Check mapping from s -> t
        if (mapST.has(charS)) {
            if (mapST.get(charS) !== charT) {
                return false;
            }
        } else {
            mapST.set(charS, charT);
        }
        
        // Check mapping from t -> s
        if (mapTS.has(charT)) {
            if (mapTS.get(charT) !== charS) {
                return false;
            }
        } else {
            mapTS.set(charT, charS);
        }
    }
    
    return true;
};
```

### Complexity Analysis
- **Time Complexity**: O(n), where n is the length of the string. We traverse the string once.
- **Space Complexity**: O(n), for storing map data based on character frequencies.

---

## Approach 3: Hash Map for Mapping + Inverse Map Check

### Intuition
This approach improves upon the previous logic by ensuring that both mappings (s -> t and t -> s) are consistent without using separate hash maps by using a single object to check inversely. This helps in validating that the characters map one-to-one.

### Implementation

```javascript
var isIsomorphic = function(s, t) {
    let mapS = new Map(); // map for storing s -> t mapping
    let mapT = new Map(); // map for storing t -> s mapping
    
    for (let i = 0; i < s.length; i++) {
        let charS = s[i];
        let charT = t[i];
        
        // Check mapping in both directions
        if ((mapS.has(charS) && mapS.get(charS) !== charT) || 
            (mapT.has(charT) && mapT.get(charT) !== charS)) {
            return false;
        }
        
        mapS.set(charS, charT);
        mapT.set(charT, charS);
    }
    
    return true;
};
```

### Complexity Analysis
- **Time Complexity**: O(n), where n is the length of the string.
- **Space Complexity**: O(n), since we store unique characters in hash maps.

