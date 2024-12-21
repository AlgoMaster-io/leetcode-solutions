
[LeetCode Problem 76: Minimum Window Substring](https://leetcode.com/problems/minimum-window-substring/)

### Approaches
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Sliding Window with Two Pointers](#approach-2-sliding-window-with-two-pointers)

---

### Approach 1: Brute Force

#### Intuition
The brute force approach involves generating all possible substrings of `s` and checking each one to see if it contains all the characters of `t`. We update the minimum window whenever we find a valid substring. This approach is straightforward but inefficient, as it checks each substring individually.

#### Solution
```typescript
function minWindow(s: string, t: string): string {
    if (s.length < t.length) return "";

    let minLength = Infinity;
    let minWindow = "";

    for (let start = 0; start < s.length; start++) {
        for (let end = start + t.length; end <= s.length; end++) {
            const substring = s.substring(start, end);

            // Check if current substring contains all characters of t
            if (containsAllChars(substring, t)) {
                if (substring.length < minLength) {
                    minLength = substring.length;
                    minWindow = substring;
                }
            }
        }
    }

    return minWindow;
}

function containsAllChars(s: string, t: string): boolean {
    const tFrequency: { [key: string]: number } = {};
    for (let char of t) {
        tFrequency[char] = (tFrequency[char] || 0) + 1;
    }

    const sFrequency: { [key: string]: number } = {};
    for (let char of s) {
        sFrequency[char] = (sFrequency[char] || 0) + 1;
    }

    for (let char of Object.keys(tFrequency)) {
        if (!sFrequency[char] || sFrequency[char] < tFrequency[char]) {
            return false;
        }
    }

    return true;
}
```

#### Time and Space Complexity
- **Time Complexity**: O(n^3), where `n` is the length of `s`. Generating all substrings takes O(n^2), and checking each substring takes O(n).
- **Space Complexity**: O(n + m), where `m` is the length of `t`, for the frequency dictionary.

---

### Approach 2: Sliding Window with Two Pointers

#### Intuition
Using a sliding window along with two pointers (start and end), we can efficiently find the minimum window substring. By expanding the end pointer, we include characters and move the start pointer to reduce the window size when the current window contains all characters of `t`. This optimizes the checking process compared to brute force.

#### Solution
```typescript
function minWindowOptimized(s: string, t: string): string {
    if (s.length < t.length) return "";

    const tFrequency: { [key: string]: number } = {};
    for (let char of t) {
        tFrequency[char] = (tFrequency[char] || 0) + 1;
    }

    const windowFrequency: { [key: string]: number } = {};
    let have = 0;
    let need = Object.keys(tFrequency).length;

    let res = [-1, -1];
    let resLength = Infinity;
    let left = 0;

    for (let right = 0; right < s.length; right++) {
        const char = s[right];
        windowFrequency[char] = (windowFrequency[char] || 0) + 1;

        if (tFrequency[char] && windowFrequency[char] === tFrequency[char]) {
            have++;
        }

        while (have === need) {
            // Update result if the current window is smaller
            if ((right - left + 1) < resLength) {
                res = [left, right];
                resLength = right - left + 1;
            }

            // Try to contract the window from the left
            windowFrequency[s[left]]--;
            if (tFrequency[s[left]] && windowFrequency[s[left]] < tFrequency[s[left]]) {
                have--;
            }
            left++;
        }
    }

    const [start, end] = res;
    return resLength !== Infinity ? s.slice(start, end + 1) : "";
}
```

#### Time and Space Complexity
- **Time Complexity**: O(n + m), where `n` is the length of `s` and `m` is the length of `t`. The window expansion and contraction are linear in `s`.
- **Space Complexity**: O(n + m), used for storing character frequencies of `s` and `t`. 

This approach efficiently finds the minimum window substring, improving both time and space usage compared to the brute force method.

