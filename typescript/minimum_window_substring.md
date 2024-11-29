# [76. Minimum Window Substring](https://leetcode.com/problems/minimum-window-substring/)

## Approach 1: Brute Force (Basic)

### Solution
typescript
```typescript
// Time Complexity: O(n^2)
// Space Complexity: O(1)
function minWindow(s: string, t: string): string {
    if (t.length > s.length) return "";

    let result = "";
    let minLength = Number.MAX_VALUE;

    // Iterate over all substrings
    for (let i = 0; i < s.length; i++) {
        for (let j = i; j < s.length; j++) {
            const sub = s.substring(i, j + 1);

            if (containsAll(sub, t)) {
                if (sub.length < minLength) {
                    minLength = sub.length;
                    result = sub;
                }
            }
        }
    }

    return result;
}

function containsAll(sub: string, t: string): boolean {
    const map: Map<string, number> = new Map();

    for (const c of t) {
        map.set(c, (map.get(c) || 0) + 1);
    }

    for (const c of sub) {
        if (map.has(c)) {
            map.set(c, map.get(c)! - 1);
            if (map.get(c) === 0) {
                map.delete(c);
            }
        }
    }

    return map.size === 0;
}
```

## Approach 2: Sliding Window with Frequency Count (Optimal)

### Solution
typescript
```typescript
// Time Complexity: O(n)
// Space Complexity: O(n)
function minWindowSliding(s: string, t: string): string {
    if (t.length > s.length) return "";

    const tFreq: Map<string, number> = new Map();
    for (const c of t) {
        tFreq.set(c, (tFreq.get(c) || 0) + 1);
    }

    const windowFreq: Map<string, number> = new Map();
    let left = 0, right = 0, matched = 0;
    let minLength = Number.MAX_VALUE;
    let start = 0;

    while (right < s.length) {
        const c = s[right];
        windowFreq.set(c, (windowFreq.get(c) || 0) + 1);

        if (tFreq.has(c) && windowFreq.get(c) === tFreq.get(c)) {
            matched++;
        }

        while (matched === tFreq.size) {
            // Update the result if this window is smaller
            if (right - left + 1 < minLength) {
                minLength = right - left + 1;
                start = left;
            }

            // Shrink the window
            const leftChar = s[left];
            if (tFreq.has(leftChar) && windowFreq.get(leftChar) === tFreq.get(leftChar)) {
                matched--;
            }
            windowFreq.set(leftChar, windowFreq.get(leftChar)! - 1);
            if (windowFreq.get(leftChar) === 0) {
                windowFreq.delete(leftChar);
            }
            left++;
        }

        right++;
    }

    return minLength === Number.MAX_VALUE ? "" : s.substring(start, start + minLength);
}
```

