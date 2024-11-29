# 49. [Group Anagrams](https://leetcode.com/problems/group-anagrams/)

## Approach 1: Sorting + HashMap

### Solution
```javascript
// Time Complexity: O(n * k * log k) where n is the number of strings and k is the average string length
// Space Complexity: O(n * k)
var groupAnagrams = function(strs) {
    // Map to group anagrams by their sorted representation
    let map = new Map();

    for (let str of strs) {
        // Sort the string to create a key for anagrams
        let sortedStr = str.split('').sort().join('');

        // Add the string to the corresponding group
        if (!map.has(sortedStr)) {
            map.set(sortedStr, []);
        }
        map.get(sortedStr).push(str);
    }

    // Return all groups of anagrams
    return Array.from(map.values());
};
```

## Approach 2: Frequency Count + HashMap

### Solution
```javascript
// Time Complexity: O(n * k) where n is the number of strings and k is the average string length
// Space Complexity: O(n * k)
var groupAnagrams = function(strs) {
    // Map to group anagrams by their frequency count representation
    let map = new Map();

    for (let str of strs) {
        // Create a frequency count array for the string
        let count = new Array(26).fill(0);
        for (let c of str) {
            count[c.charCodeAt(0) - 'a'.charCodeAt(0)]++;
        }

        // Convert frequency array to a string key
        let key = count.join('#');

        // Add the string to the corresponding group
        if (!map.has(key)) {
            map.set(key, []);
        }
        map.get(key).push(str);
    }

    // Return all groups of anagrams
    return Array.from(map.values());
};
```

## Approach 3: Prime Product Hashing (Alternative)

### Solution
```javascript
// Time Complexity: O(n * k) where n is the number of strings and k is the average string length
// Space Complexity: O(n * k)
var groupAnagrams = function(strs) {
    // Assign a unique prime number to each character
    let primes = [2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37, 41, 43, 47, 
                  53, 59, 61, 67, 71, 73, 79, 83, 89, 97, 101];
    let map = new Map();

    for (let str of strs) {
        let product = 1;
        for (let c of str) {
            product *= primes[c.charCodeAt(0) - 'a'.charCodeAt(0)]; // Compute unique hash using prime products
        }

        // Add the string to the corresponding group
        if (!map.has(product)) {
            map.set(product, []);
        }
        map.get(product).push(str);
    }

    // Return all groups of anagrams
    return Array.from(map.values());
};
```

