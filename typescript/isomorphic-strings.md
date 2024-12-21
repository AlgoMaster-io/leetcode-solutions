# [205. Isomorphic Strings](https://leetcode.com/problems/isomorphic-strings/)

In this problem, we are required to determine whether two given strings are isomorphic. Two strings are isomorphic if the characters in one string can be replaced to get the other string, while consistently maintaining the character mapping. Different characters should map to different characters.

## Approaches:
- [Approach 1: Character Mapping with Two Hash Maps](#approach-1-character-mapping-with-two-hash-maps)
- [Approach 2: Character Mapping with Single HashMap and Set](#approach-2-character-mapping-with-single-hashmap-and-set)
- [Approach 3: Encoding Pattern](#approach-3-encoding-pattern)

### Approach 1: Character Mapping with Two Hash Maps

**Intuition**:  
We can utilize two hash maps to keep track of the character mappings between the two strings. The first hash map will store the mapping from the characters in the first string to the characters in the second string. The second hash map will ensure that no two different characters from the first string map to the same character in the second string.

```typescript
function isIsomorphic(s: string, t: string): boolean {
    const mapST = new Map<string, string>();
    const mapTS = new Map<string, string>();

    for (let i = 0; i < s.length; i++) {
        const charS = s[i];
        const charT = t[i];
        
        // Check consistent mapping from s -> t
        if (mapST.has(charS)) {
            if (mapST.get(charS) !== charT) {
                return false;
            }
        } else {
            mapST.set(charS, charT);
        }

        // Check consistent mapping from t -> s
        if (mapTS.has(charT)) {
            if (mapTS.get(charT) !== charS) {
                return false;
            }
        } else {
            mapTS.set(charT, charS);
        }
    }
    return true;
}
```
**Time Complexity**: O(n), where n is the length of the strings.  
**Space Complexity**: O(n), due to storage in two hash maps.

### Approach 2: Character Mapping with Single HashMap and Set

**Intuition**:  
Instead of maintaining two hash maps, we can optimize space usage by using one hash map for character mapping and a set to check whether a character from the second string is already mapped to another character.

```typescript
function isIsomorphic(s: string, t: string): boolean {
    const mapS = new Map<string, string>();
    const mappedChars = new Set<string>();

    for (let i = 0; i < s.length; i++) {
        const charS = s[i];
        const charT = t[i];

        // Check if there is a mapping for s character
        if (mapS.has(charS)) {
            if (mapS.get(charS) !== charT) {
                return false;
            }
        } else {
            // Ensure charT is not already mapped
            if (mappedChars.has(charT)) {
                return false;
            }
            mapS.set(charS, charT);
            mappedChars.add(charT);
        }
    }
    return true;
}
```
**Time Complexity**: O(n), where n is the length of the strings.  
**Space Complexity**: O(n), due to storage in a hash map and a set.

### Approach 3: Encoding Pattern

**Intuition**:  
Another approach to determine whether two strings are isomorphic is to encode the pattern of appearance of characters in both strings. If both strings share the same pattern, then they are isomorphic.

```typescript
function encodeStringPattern(str: string): string {
    const map = new Map<string, number>();
    let pattern = "";
    let id = 0;

    for (const char of str) {
        if (!map.has(char)) {
            map.set(char, id++);
        }
        pattern += map.get(char) + ",";
    }
    return pattern;
}

function isIsomorphic(s: string, t: string): boolean {
    return encodeStringPattern(s) === encodeStringPattern(t);
}
```
**Time Complexity**: O(n), where n is the length of the strings.  
**Space Complexity**: O(n), due to storage in a hash map and the generated pattern string.

