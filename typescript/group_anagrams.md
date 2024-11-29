# [49. Group Anagrams](https://leetcode.com/problems/group-anagrams/)

## Approach 1: Sorting + HashMap

### Solution
typescript
```typescript
// Time Complexity: O(n * k * log k) where n is the number of strings and k is the average string length
// Space Complexity: O(n * k)
function groupAnagrams(strs: string[]): string[][] {
    const map: Map<string, string[]> = new Map();

    for (const str of strs) {
        // Sort the string to create a key for anagrams
        const sortedStr: string = str.split('').sort().join('');

        // Add the string to the corresponding group
        if (!map.has(sortedStr)) {
            map.set(sortedStr, []);
        }
        map.get(sortedStr)!.push(str);
    }

    // Return all groups of anagrams
    return Array.from(map.values());
}
```

## Approach 2: Frequency Count + HashMap

### Solution
typescript
```typescript
// Time Complexity: O(n * k) where n is the number of strings and k is the average string length
// Space Complexity: O(n * k)
function groupAnagrams(strs: string[]): string[][] {
    const map: Map<string, string[]> = new Map();

    for (const str of strs) {
        // Create a frequency count array for the string
        const count: number[] = new Array(26).fill(0);
        for (const c of str) {
            count[c.charCodeAt(0) - 'a'.charCodeAt(0)]++;
        }

        // Convert frequency array to a string key
        const key = count.join('#');

        // Add the string to the corresponding group
        if (!map.has(key)) {
            map.set(key, []);
        }
        map.get(key)!.push(str);
    }

    // Return all groups of anagrams
    return Array.from(map.values());
}
```

## Approach 3: Prime Product Hashing (Alternative)

### Solution
typescript
```typescript
// Time Complexity: O(n * k) where n is the number of strings and k is the average string length
// Space Complexity: O(n * k)
function groupAnagrams(strs: string[]): string[][] {
    // Assign a unique prime number to each character
    const primes: number[] = [2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37, 41, 43, 47, 
                              53, 59, 61, 67, 71, 73, 79, 83, 89, 97, 101];
    const map: Map<number, string[]> = new Map();

    for (const str of strs) {
        let product: number = 1;

        for (const c of str) {
            product *= primes[c.charCodeAt(0) - 'a'.charCodeAt(0)]; // Compute unique hash using prime products
        }

        // Add the string to the corresponding group
        if (!map.has(product)) {
            map.set(product, []);
        }
        map.get(product)!.push(str);
    }

    // Return all groups of anagrams
    return Array.from(map.values());
}
```

