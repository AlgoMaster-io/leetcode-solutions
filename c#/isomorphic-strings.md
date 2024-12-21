# [205. Isomorphic Strings](https://leetcode.com/problems/isomorphic-strings/)

## Approaches
- [Approach 1: Brute Force using two hash maps](#approach-1)
- [Approach 2: Efficient Approach using a single index map](#approach-2)

---

## Approach 1: Brute Force using two hash maps

### Intuition
To determine if two strings are isomorphic, we need to ensure that each character in the first string can be mapped to a character in the second string without conflict. The simplest way is to use two hash maps, one to map characters from the first string to the second and another to check the reverse mapping. This ensures that each character in both strings maintains a consistent mapping.

### Implementation

```csharp
public bool IsIsomorphic(string s, string t) {
    // Maps to track character to character mapping from s to t and t to s
    Dictionary<char, char> mapST = new Dictionary<char, char>();
    Dictionary<char, char> mapTS = new Dictionary<char, char>();
    
    for (int i = 0; i < s.Length; i++) {
        char charS = s[i];
        char charT = t[i];
        
        // Check if there's already a mapping established for charS in mapST
        if (mapST.ContainsKey(charS)) {
            // If there's a conflict in mapping, return false
            if (mapST[charS] != charT) {
                return false;
            }
        } else {
            mapST[charS] = charT;
        }
        
        // Similarly, check mapping for charT in mapTS
        if (mapTS.ContainsKey(charT)) {
            if (mapTS[charT] != charS) {
                return false;
            }
        } else {
            mapTS[charT] = charS;
        }
    }
    
    return true;
}
```

### Time Complexity
- **O(n)**: We traverse the strings once.

### Space Complexity
- **O(n)**: In the worst case, we store every character mapping in the hash maps.

---

## Approach 2: Efficient Approach using a single index map

### Intuition
The previous approach used two maps to ensure bi-directional consistency. However, we can optimize it to use a single map by storing encoded transformations of each character for both strings and comparing these transformations. By mapping each character to its first occurrence index, we can uniquely codify its transformation in each string.

### Implementation

```csharp
public bool IsIsomorphic(string s, string t) {
    int[] mapS = new int[256]; // to store index of first occurrence of each character in s
    int[] mapT = new int[256]; // to store index of first occurrence of each character in t
    
    for (int i = 0; i < s.Length; i++) {
        // If the characters match the same transformation in both s and t
        if (mapS[s[i]] != mapT[t[i]]) {
            return false; // mismatch found
        }
        
        // Store index of first occurrence as i+1 (using i+1 to avoid initial zero comparison uncertainty)
        mapS[s[i]] = i + 1;
        mapT[t[i]] = i + 1;
    }
    
    return true;
}
```

### Time Complexity
- **O(n)**: Still traversing both strings once.

### Space Complexity
- **O(1)**: Utilizing fixed size arrays independent of input size.


