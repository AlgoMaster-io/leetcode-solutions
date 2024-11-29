# 686. [Repeated String Match](https://leetcode.com/problems/repeated-string-match/)

## Approach 1: Brute Force

### Solution
typescript
```typescript
// Time Complexity: O(n * m), where n is the length of A and m is the length of B
// Space Complexity: O(n + m)
function repeatedStringMatch(A: string, B: string): number {
    let repeatedA = A;
    let repeatCount = 1;

    // Repeat A until its length is at least the length of B
    while (repeatedA.length < B.length) {
        repeatedA += A;
        repeatCount++;
    }

    // Check if B is a substring of the repeated A
    if (repeatedA.includes(B)) {
        return repeatCount;
    }

    // Add one more repetition just in case B starts towards the end
    repeatedA += A;
    if (repeatedA.includes(B)) {
        return repeatCount + 1;
    }

    return -1; // B is not a substring of repeated A
}
```

## Approach 2: Optimized Approach using Rolling Window

### Solution
typescript
```typescript
// Time Complexity: O(n + m), where n is the length of A and m is the length of B
// Space Complexity: O(n + m)
function repeatedStringMatch(A: string, B: string): number {
    const m = A.length;
    const n = B.length;
    const maxRepeat = Math.floor(n / m) + 2; // Only need to repeat at most this many times

    let repeatedA = "";
    for (let i = 0; i < maxRepeat; i++) {
        repeatedA += A;
        // Check if B is a substring of the current repeated A
        if (repeatedA.includes(B)) {
            return i + 1;
        }
    }

    return -1; // B is not a substring of repeated A
}
```

