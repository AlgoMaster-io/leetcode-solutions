# [Leetcode 383: Ransom Note](https://leetcode.com/problems/ransom-note/)

## Table of Contents
1. [Approach 1: Brute Force / Naive Approach](#approach-1-brute-force--naive-approach)
2. [Approach 2: HashMap Counting](#approach-2-hashmap-counting)

---

## Approach 1: Brute Force / Naive Approach

### Intuition
The basic approach to solve this problem is to use a double loop. For each character in the ransom note, we look for that character in the magazine and remove it once found. This approach simulates the manual process of cutting out words from a magazine to form a ransom note.

### Steps
1. Convert the `magazine` string to an array, as strings are immutable in JavaScript.
2. Iterate through each character in the `ransomNote`.
3. For each character, find its index in the `magazine` array.
4. If found, remove the character from the `magazine` array so it won't be reused.
5. If any character cannot be found, return false.
6. If the loop completes, return true.

### Code

```javascript
function canConstruct(ransomNote, magazine) {
    let magazineArr = magazine.split(''); // Convert magazine to array
    for (let char of ransomNote) {
        let index = magazineArr.indexOf(char); // Find index of char in magazine
        if (index === -1) {
            return false;  // Char not found, return false
        }
        magazineArr.splice(index, 1); // Remove the found char
    }
    return true; // All chars found, return true
}
```

### Complexity Analysis
- **Time Complexity**: O(n * m), where n is the length of the ransom note and m is the length of the magazine. In the worst case, we have to search for each character in the whole magazine.
- **Space Complexity**: O(m), where m is the length of the magazine. This is due to splitting the magazine into an array.

---

## Approach 2: HashMap Counting

### Intuition
A more efficient way is to use a HashMap (or object in JavaScript) to count the occurrences of each character in the magazine. We then iterate over the ransom note and check if we can use the counted characters to "construct" the ransom note.

### Steps
1. Create a HashMap to store the frequency of each character from the `magazine`.
2. For each character in the `magazine`, increment its count in the HashMap.
3. Iterate through each character in the `ransomNote`.
4. For each character, check its frequency in the HashMap.
5. If the character is not present or its frequency is zero, return false.
6. Decrement the character's frequency in the HashMap.
7. If the loop completes, return true.

### Code

```javascript
function canConstruct(ransomNote, magazine) {
    const magazineCount = {}; // Frequency map for magazine characters
    
    for (let char of magazine) {
        // Increment count for each character
        magazineCount[char] = (magazineCount[char] || 0) + 1;
    }
    
    for (let char of ransomNote) {
        if (!magazineCount[char] || magazineCount[char] <= 0) {
            return false; // Char not available or exhausted, return false
        }
        magazineCount[char]--; // Decrement the count as it's used
    }
    
    return true; // All chars found and used, return true
}
```

### Complexity Analysis
- **Time Complexity**: O(n + m), where n is the length of the ransom note and m is the length of the magazine. We go through each once.
- **Space Complexity**: O(m), where m is the length of the magazine in the worst case, because we store each character in a HashMap. 

This approach is more optimal compared to the brute force, especially for large inputs, since it efficiently counts and checks each character only once.

