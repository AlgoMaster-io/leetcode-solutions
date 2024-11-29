# [1189. Maximum Number of Balloons](https://leetcode.com/problems/maximum-number-of-balloons/)

## Approach 1: Frequency Count with HashMap

### Solution
```javascript
// Time Complexity: O(n)
// Space Complexity: O(1)
function maxNumberOfBalloons(text) {
    const countMap = new Map();

    // Count frequencies of each character in the text
    for (const c of text) {
        countMap.set(c, (countMap.get(c) || 0) + 1);
    }

    // Check the minimum occurrences of characters required for "balloon"
    const b = countMap.get('b') || 0;
    const a = countMap.get('a') || 0;
    const l = Math.floor((countMap.get('l') || 0) / 2); // 'l' appears twice in "balloon"
    const o = Math.floor((countMap.get('o') || 0) / 2); // 'o' appears twice in "balloon"
    const n = countMap.get('n') || 0;

    // Return the minimum number of complete "balloon" words
    return Math.min(b, a, l, o, n);
}
```

## Approach 2: Frequency Count with Array

### Solution
```javascript
// Time Complexity: O(n)
// Space Complexity: O(1)
function maxNumberOfBalloons(text) {
    const charCount = new Array(26).fill(0); // Frequency array for all characters

    // Count frequencies of each character in the text
    for (const c of text) {
        charCount[c.charCodeAt(0) - 'a'.charCodeAt(0)]++;
    }

    // Check the minimum occurrences of characters required for "balloon"
    const b = charCount['b'.charCodeAt(0) - 'a'.charCodeAt(0)];
    const a = charCount['a'.charCodeAt(0) - 'a'.charCodeAt(0)];
    const l = Math.floor(charCount['l'.charCodeAt(0) - 'a'.charCodeAt(0)] / 2); // 'l' appears twice in "balloon"
    const o = Math.floor(charCount['o'.charCodeAt(0) - 'a'.charCodeAt(0)] / 2); // 'o' appears twice in "balloon"
    const n = charCount['n'.charCodeAt(0) - 'a'.charCodeAt(0)];

    // Return the minimum number of complete "balloon" words
    return Math.min(b, a, l, o, n);
}
```

## Approach 3: Optimized Single Pass

### Solution
```javascript
// Time Complexity: O(n)
// Space Complexity: O(1)
function maxNumberOfBalloons(text) {
    const counts = new Array(5).fill(0); // Indices: 0->b, 1->a, 2->l, 3->o, 4->n

    // Count only relevant characters
    for (const c of text) {
        switch (c) {
            case 'b': counts[0]++; break;
            case 'a': counts[1]++; break;
            case 'l': counts[2]++; break;
            case 'o': counts[3]++; break;
            case 'n': counts[4]++; break;
        }
    }

    // 'l' and 'o' need to be divided by 2 as they appear twice in "balloon"
    counts[2] = Math.floor(counts[2] / 2);
    counts[3] = Math.floor(counts[3] / 2);

    // Return the minimum count
    return Math.min(...counts);
}
```

