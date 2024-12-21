[Leetcode 383: Ransom Note](https://leetcode.com/problems/ransom-note/)

### Approaches
1. [Basic Approach - Array Frequency Count](#basic-approach---array-frequency-count)
2. [Optimized Approach - Hash Map](#optimized-approach---hash-map)

---

### Basic Approach - Array Frequency Count

**Intuition:**  
The basic idea to solve this problem is to verify if the `magazine` contains all the characters necessary to construct the `ransom` note. We can achieve this by using a frequency count array, since both the ransom note and magazine contain lowercase letters, which map nicely to an array of size 26.

In this approach, we need to count the frequency of each character in the `magazine` and then iterate over the `ransomNote` to see if its characters can be satisfied by the `magazine`.

**Steps:**
1. Initialize an array `charCount` of size 26 to zero, which represents counts of each character from 'a' to 'z'.
2. Iterate over each character in the `magazine` and increment its corresponding position in the `charCount`.
3. Iterate over each character in the `ransomNote` and decrement its corresponding count in `charCount`.
4. If the count goes below zero for any character, return `false` as `magazine` does not have enough of that character to satisfy the `ransomNote`.
5. If all characters in `ransomNote` are satisfied, return `true`.

**Time Complexity:** O(m + n), where m is the length of `magazine` and n is the length of `ransomNote`.  
**Space Complexity:** O(1), since the array size is fixed to 26.

```typescript
function canConstruct(ransomNote: string, magazine: string): boolean {
    const charCount: number[] = new Array(26).fill(0);
    
    // Fill the frequency array with magazine character counts
    for (let ch of magazine) {
        charCount[ch.charCodeAt(0) - 'a'.charCodeAt(0)]++;
    }
    
    // Check if ransomNote can be constructed using magazine characters
    for (let ch of ransomNote) {
        const index = ch.charCodeAt(0) - 'a'.charCodeAt(0);
        if (charCount[index] <= 0) {
            return false;  // If character frequency goes negative, return false
        }
        charCount[index]--;
    }
    
    return true;
}
```

---

### Optimized Approach - Hash Map

**Intuition:**  
Using a hashmap gives us a bit more flexibility and direct manipulation of counts without relying on array indexing. This approach essentially mirrors the above but with a hashmap structure to count the `magazine` characters and check against the `ransomNote`.

**Steps:**
1. Use a map `charMap` to count occurrences of each character in `magazine`.
2. For each character in `ransomNote`, check if it exists in the `charMap` with a positive count.
3. Decrease the count of the character in `charMap` if possible.
4. If the character's count in `charMap` is zero or doesn't exist, return `false`.
5. If all characters are covered, return `true`.

**Time Complexity:** O(m + n), where m is the length of `magazine` and n is the length of `ransomNote`.  
**Space Complexity:** O(k), where k is the number of distinct characters in `magazine`, though it caps at 26.

```typescript
function canConstructHashMap(ransomNote: string, magazine: string): boolean {
    const charMap: Map<string, number> = new Map();
    
    // Populate character counts from the magazine
    for (let ch of magazine) {
        charMap.set(ch, (charMap.get(ch) || 0) + 1);
    }
    
    // Check if ransomNote can be constructed using magazine characters
    for (let ch of ransomNote) {
        if (!charMap.has(ch) || charMap.get(ch)! <= 0) {
            return false;  // If no occurrence left for a character, return false
        }
        charMap.set(ch, charMap.get(ch)! - 1);
    }
    
    return true;
}
```

Both approaches ensure that each character required by the `ransomNote` is available in sufficient quantity from the `magazine`. The array-based solution is slightly more efficient in terms of space, due to fixed constant usage, while the hashmap offers readability and slightly more adaptable handling of input characters.

