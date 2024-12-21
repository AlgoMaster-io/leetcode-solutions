# [Leetcode 686: Repeated String Match](https://leetcode.com/problems/repeated-string-match/)

## Approaches
1. [Brute Force Approach](#approach-1)
2. [Optimized Approach Using String Methods](#approach-2)
3. [Even More Optimized Approach Using KMP Algorithm](#approach-3)

### Approach 1: Brute Force Approach
In the brute force approach, we repeatedly concatenate the string `A` until the length of the repeated string is at least as large as the string `B`. After that, we keep adding the string `A` until the concatenated string is no longer needed and check if `B` is a substring.

#### Intuition:
- Start by creating a repeated string `A` that is initially just `A`.
- Gradually concatenate `A` to itself until the repeated length is >= `B.length`.
- Check if `B` is a substring of this repeated string.
- Continue to add `A` if necessary and check again.
- Return the number of concatenations needed or -1 if `B` never becomes a substring.

#### Code:
```javascript
function repeatedStringMatch(A, B) {
    // Start with one instance of A
    let repeatedA = A;
    // Calculate the number of times we need to repeat A for the length to be at least B's length
    let count = Math.ceil(B.length / A.length);
    
    // Repeat A `count` times to start with
    repeatedA = A.repeat(count);
    
    // Check our repeated A
    if (repeatedA.includes(B)) return count;
    
    // Check for one extra repetition of A to cover edge cases
    repeatedA += A;
    if (repeatedA.includes(B)) return count + 1;
    
    // If neither works, return -1
    return -1;
}
```

#### Complexity Analysis:
- **Time Complexity**: O(n * m), where n is the length of `B` and m is the length of `A`. We are checking if `B` is a substring of the repeated `A`.
- **Space Complexity**: O(n + m) for the space required to store the repeated string.

### Approach 2: Optimized Approach Using String Methods
This approach leverages directly checking the required repetitions and making use of string concatenation and methods efficiently.

#### Intuition:
- It starts similar to the brute force but uses a bit of optimization for checks.
- Directly check up to two repetitions beyond the minimum required repeats.
- Use direct string methods like `indexOf` to check for substring presence.

#### Code:
```javascript
function repeatedStringMatch(A, B) {
    // The minimum repeats needed 
    let minRepeats = Math.ceil(B.length / A.length);

    // Create a repeated string with A repeated minRepeats
    let repeatedA = A.repeat(minRepeats);
    
    // Check if B is in the minReps string
    if (repeatedA.indexOf(B) !== -1) return minRepeats;
    
    // Check one more repeat 
    repeatedA += A;
    if (repeatedA.indexOf(B) !== -1) return minRepeats + 1;
    
    return -1;
}
```

#### Complexity Analysis:
- **Time Complexity**: O(n + m), primarily dominated by the indexOf check which is efficient for strings.
- **Space Complexity**: O(n + m) for the construction of repeated strings.

### Approach 3: Even More Optimized Approach Using KMP Algorithm
To further optimize, we employ the KMP (Knuth-Morris-Pratt) algorithm to efficiently check for substring presence. 

#### Intuition:
- Use KMP to preprocess the pattern `B` to create a longest prefix-suffix (lps) table.
- Utilize the lps table for efficient matching.
- This approach is efficient in scenarios where repeating and checking becomes costly with larger strings.

#### Code:
```javascript
function repeatedStringMatch(A, B) {
    const n = A.length, m = B.length;

    // KMP preprocessing for pattern B
    function kmpProcess(B) {
      const lps = Array(m).fill(0);
      let len = 0, i = 1;
      while (i < m) {
        if (B[i] === B[len]) {
          len++;
          lps[i] = len;
          i++;
        } else {
          if (len !== 0) {
            len = lps[len - 1];
          } else {
            lps[i] = 0;
            i++;
          }
        }
      }
      return lps;
    }

    const lps = kmpProcess(B);

    // KMP search for B in repeated A
    function kmpSearch(text, pattern, lps) {
      const n = text.length;
      const m = pattern.length;
      let i = 0, j = 0;
      while (i < n) {
        if (pattern[j] === text[i]) {
          i++;
          j++;
        }
        if (j === m) {
          return true;
        } else if (i < n && pattern[j] !== text[i]) {
          if (j !== 0) {
            j = lps[j - 1];
          } else {
            i++;
          }
        }
      }
      return false;
    }

    let repeatedA = A.repeat(Math.ceil(m / n));
    let count = Math.ceil(m / n);

    if (kmpSearch(repeatedA, B, lps)) return count;

    repeatedA += A;
    if (kmpSearch(repeatedA, B, lps)) return count + 1;

    return -1;
}
```

#### Complexity Analysis:
- **Time Complexity**: O(n + m), where KMP preprocessing is O(m) and searching is O(n).
- **Space Complexity**: O(m) for the lps array used in the KMP algorithm.

