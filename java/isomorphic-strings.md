# [205. Isomorphic Strings](https://leetcode.com/problems/isomorphic-strings/)

## Solutions
1. [Brute Force Approach](#brute-force-approach)
2. [Using Hash Maps](#using-hash-maps)

---

### Brute Force Approach

Intuition:
A brute force approach involves checking character by character and ensuring each character maps to exactly one character. For this, we can use an array to track the mapping of characters.

- For each character in the given strings, map them into a fixed-size array based on ASCII values.
- Check if there is a consistent mapping from characters of `s` to `t` and `t` to `s`.

#### Code
```java
public boolean isIsomorphic(String s, String t) {
    if (s.length() != t.length()) return false;
    
    int[] mapS = new int[256];
    int[] mapT = new int[256];
    
    for (int i = 0; i < s.length(); i++) {
        char charS = s.charAt(i);
        char charT = t.charAt(i);
        
        // Check if previous mapped characters are same
        if (mapS[charS] != mapT[charT]) return false;
        
        // Map characters to index+1 to avoid default 0 confusion
        mapS[charS] = i + 1;
        mapT[charT] = i + 1;
    }
    
    return true;
}
```

#### Complexity Analysis
- **Time Complexity**: O(n), where n is the length of the string.
  - We iterate over the string once.
- **Space Complexity**: O(1).
  - The space used by the arrays is constant due to the fixed size (256).

---

### Using Hash Maps

Intuition:
To better understand the character relationships, we'll use two hash maps. This solution tracks mapping from both `s` to `t` and `t` to `s`, ensuring a one-to-one and onto mapping.

- Create two hash maps.
- For each character, ensure the mapping is unique and consistent in both directions.

#### Code
```java
import java.util.HashMap;
import java.util.Map;

public boolean isIsomorphic(String s, String t) {
    if (s.length() != t.length()) return false;
    
    Map<Character, Character> mapST = new HashMap<>();
    Map<Character, Character> mapTS = new HashMap<>();
    
    for (int i = 0; i < s.length(); i++) {
        char charS = s.charAt(i);
        char charT = t.charAt(i);
        
        // Check mapping from s to t
        if (mapST.containsKey(charS)) {
            if (mapST.get(charS) != charT) return false;
        } else {
            mapST.put(charS, charT);
        }
        
        // Check mapping from t to s
        if (mapTS.containsKey(charT)) {
            if (mapTS.get(charT) != charS) return false;
        } else {
            mapTS.put(charT, charS);
        }
    }
    
    return true;
}
```

#### Complexity Analysis
- **Time Complexity**: O(n), where n is the length of the string.
  - We iterate through each character once.
- **Space Complexity**: O(1), specifically O(26) if considering distinct lowercase letters.
  - In practice, the space used by hash maps is relative to the character set.

