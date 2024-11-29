# [3. Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters/)

## Approach 1: Brute Force (Basic)

### Solution
typescript
```typescript
// Time Complexity: O(n^2)
// Space Complexity: O(n)
function lengthOfLongestSubstring(s: string): number {
    let maxLength = 0;

    // Check every possible substring
    for (let i = 0; i < s.length; i++) {
        const seen = new Set<string>();
        let length = 0;

        for (let j = i; j < s.length; j++) {
            if (seen.has(s[j])) {
                break; // Break if a duplicate character is found
            }
            seen.add(s[j]);
            length++;
            maxLength = Math.max(maxLength, length); // Update maxLength
        }
    }

    return maxLength;
}
```

## Approach 2: Sliding Window with HashSet

### Solution
typescript
```typescript
// Time Complexity: O(n)
// Space Complexity: O(n)
function lengthOfLongestSubstring(s: string): number {
    const set = new Set<string>();
    let maxLength = 0;
    let left = 0;

    // Sliding window
    for (let right = 0; right < s.length; right++) {
        while (set.has(s[right])) {
            set.delete(s[left]); // Shrink the window
            left++;
        }
        set.add(s[right]); // Expand the window
        maxLength = Math.max(maxLength, right - left + 1); // Update maxLength
    }

    return maxLength;
}
```

## Approach 3: Sliding Window with HashMap (Optimal)

### Solution
typescript
```typescript
// Time Complexity: O(n)
// Space Complexity: O(n)
function lengthOfLongestSubstring(s: string): number {
    const map = new Map<string, number>();
    let maxLength = 0;
    let left = 0;

    // Sliding window
    for (let right = 0; right < s.length; right++) {
        const currentChar = s[right];

        // If the character is already in the map, update the left pointer
        if (map.has(currentChar)) {
            left = Math.max(left, map.get(currentChar)! + 1);
        }

        // Update the character's last index in the map
        map.set(currentChar, right);

        // Update maxLength
        maxLength = Math.max(maxLength, right - left + 1);
    }

    return maxLength;
}
```

