[Leetcode Problem 686: Repeated String Match](https://leetcode.com/problems/repeated-string-match/)

## Approaches
1. [Brute Force](#brute-force)
2. [Optimized Approach using Math](#optimized-approach-using-math)

---

### Brute Force

**Intuition:**
The basic approach is to keep appending the string `A` to itself and check if this repeated version contains the string `B`. Continue this repetitive process until the length of the repeated string is at least as long as `B`. This ensures that all necessary combinations are covered.

**Steps:**
1. Create a new string by repeating `A` and check each time if it contains `B`.
2. Keep track of how many times you repeat `A`.
3. Stop when the repeated string is longer than `B`.

**Code:**

```typescript
function repeatedStringMatch(A: string, B: string): number {
    let repeated = A;
    let count = 1;

    // Repeat A until its length is at least as long as B
    while (repeated.length < B.length) {
        repeated += A;
        count++;
    }

    // Check if B is now a substring of the repeated string
    if (repeated.indexOf(B) !== -1) {
        return count;
    }

    // Add one more repetition to cover the edge case for overlapping scenario
    repeated += A;
    count++;

    return repeated.indexOf(B) !== -1 ? count : -1; // Return -1 if B is not found
}
```

**Time Complexity:** O(n * m), where n is the length of `A` and m is the length of `B`.

**Space Complexity:** O(n + m) for the repeated string.

---

### Optimized Approach using Math

**Intuition:**
A more efficient approach leverages the properties of string length. The minimum number of times `A` needs to be repeated to be longer than `B` is `Math.ceil(B.length / A.length)`. This provides a baseline. Additionally, checking for one more repeat is needed to account for cases where `B` might start towards the end of one `A` but finish at the start of the next. 

**Steps:**
1. Calculate the minimum number of repetitions needed using `Math.ceil(B.length / A.length)`.
2. Check the substring for this and one additional repetition.

**Code:**

```typescript
function repeatedStringMatch(A: string, B: string): number {
    const minRepeats = Math.ceil(B.length / A.length);
    let repeated = A.repeat(minRepeats);

    // Check if B is a substring of the minimal repeat
    if (repeated.indexOf(B) !== -1) {
        return minRepeats;
    }

    // Check if B is a substring when we add one more A
    repeated += A;
    if (repeated.indexOf(B) !== -1) {
        return minRepeats + 1;
    }

    // If not found, return -1
    return -1;
}
```

**Time Complexity:** O(n + m), similar lengths of `A` and `B` are tested with fewer repetitions.

**Space Complexity:** O(n + m), for the repeated string.

---

This problem benefits significantly from realizing the minimal repeating properties and efficiently handling the potential overlap of `B` at the boundaries of repeated `A`.

