### [Leetcode Problem 132: Palindrome Partitioning II](https://leetcode.com/problems/palindrome-partitioning-ii/)

## Approaches

1. [Dynamic Programming with Palindrome Check](#dynamic-programming-with-palindrome-check)
2. [Optimized Dynamic Programming](#optimized-dynamic-programming)

---

### 1. Dynamic Programming with Palindrome Check

#### Intuition
The main idea is to use dynamic programming to keep track of the minimum cuts needed for a substring. We start by calculating if any substring is a palindrome and then use this information to determine the minimum number of cuts needed for each substring.

#### Approach
- Use a 2D boolean matrix `palindrome` where `palindrome[i][j] = true` if the substring `s[i...j]` is a palindrome.
- Use a 1D array `cut` where `cut[i]` holds the minimum cuts needed for the substring `s[0...i]`.
- Populate the `palindrome` matrix using the conditions: 
  - `s[i] == s[j]` and `palindrome[i+1][j-1] == true` if the substring is longer than two.
- For each position `i`, calculate the minimum cuts by checking if any substring `s[j...i]` is a palindrome and update `cut[i]` accordingly.

#### Java Code

```java
public int minCut(String s) {
    int n = s.length();
    boolean[][] palindrome = new boolean[n][n];
    int[] cut = new int[n];

    for (int i = 0; i < n; i++) {
        int minCuts = i; // maximum cuts can be i (i.e., cut each character)
        for (int j = 0; j <= i; j++) {
            // Check if s[j..i] is a palindrome
            if (s.charAt(j) == s.charAt(i) && (i - j < 2 || palindrome[j + 1][i - 1])) {
                palindrome[j][i] = true;
                // If s[0..i] is a palindrome, no need to cut; otherwise cut at j
                minCuts = j == 0 ? 0 : Math.min(minCuts, cut[j - 1] + 1);
            }
        }
        cut[i] = minCuts;
    }
    return cut[n - 1];
}
```

#### Time and Space Complexities
- **Time Complexity:** `O(n^2)`, where `n` is the length of the string. We are filling up an `n x n` matrix.
- **Space Complexity:** `O(n^2)` for the palindrome matrix.

---

### 2. Optimized Dynamic Programming

#### Intuition
We can optimize the solution by reducing unnecessary recalculations and using a more memory-efficient way to determine palindrome substrings.

#### Approach
- Instead of using a 2D array to track palindromes, we maintain one array for storing minimum cuts and use an inner loop to decide if a substring is a palindrome on the fly.
- Use two loops to handle both even and odd length center expansions for palindrome checking.

#### Java Code

```java
public int minCut(String s) {
    int n = s.length();
    int[] cut = new int[n];
    for (int i = 0; i < n; i++) {
        cut[i] = i; // max cuts equal to length - 1
    }
    
    for (int center = 0; center < n; center++) {
        // Odd-length palindrome check
        for (int j = 0; center - j >= 0 && center + j < n && s.charAt(center - j) == s.charAt(center + j); j++) {
            if (center - j == 0) {
                cut[center + j] = 0;
            } else {
                cut[center + j] = Math.min(cut[center + j], cut[center - j - 1] + 1);
            }
        }
        // Even-length palindrome check
        for (int j = 0; center - j >= 0 && center + j + 1 < n && s.charAt(center - j) == s.charAt(center + j + 1); j++) {
            if (center - j == 0) {
                cut[center + j + 1] = 0;
            } else {
                cut[center + j + 1] = Math.min(cut[center + j + 1], cut[center - j - 1] + 1);
            }
        }
    }
    return cut[n - 1];
}
```

#### Time and Space Complexities
- **Time Complexity:** `O(n^2)`, still iterating potential palindrome centers and expanding.
- **Space Complexity:** `O(n)`, no extra 2D array. Only using the `cut` array for space.

