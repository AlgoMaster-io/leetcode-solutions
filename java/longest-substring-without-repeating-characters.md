# [Leetcode Problem 3: Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters/)

### Approaches:
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Sliding Window Using Set](#approach-2-sliding-window-using-set)
- [Approach 3: Optimized Sliding Window Using Map](#approach-3-optimized-sliding-window-using-map)

## Approach 1: Brute Force

### Intuition
The most straightforward way to solve this problem is by checking each possible substring to see if it contains all unique characters. We can iterate over all possible starting points and ending points for substrings and check for uniqueness.

### Implementation
```java
public int lengthOfLongestSubstring(String s) {
    int n = s.length();
    int maxLength = 0;
    
    for (int i = 0; i < n; i++) {
        for (int j = i; j < n; j++) {
            if (isUnique(s, i, j)) {
                maxLength = Math.max(maxLength, j - i + 1);
            }
        }
    }
    
    return maxLength;
}

private boolean isUnique(String s, int start, int end) {
    // Use a set to track characters in the current substring
    Set<Character> chars = new HashSet<>();
    
    for (int k = start; k <= end; k++) {
        // If character is already in the set, it's not unique
        if (chars.contains(s.charAt(k))) {
            return false;
        }
        chars.add(s.charAt(k));
    }
    return true;
}
```

### Complexity Analysis
- **Time Complexity:** O(n^3). There are two nested loops and checking whether a substring contains all unique characters is O(n).
- **Space Complexity:** O(min(n, m)), where n is the length of the string and m is the character set size (limited to 26 for this problem).

## Approach 2: Sliding Window Using Set

### Intuition
To reduce the complexity, we can use a sliding window technique. Instead of checking every substring, we can maintain a window and move it to the right while ensuring all characters within the window are unique.

### Implementation
```java
public int lengthOfLongestSubstring(String s) {
    int n = s.length();
    int maxLength = 0;
    int i = 0, j = 0;
    Set<Character> charSet = new HashSet<>();
    
    while (i < n && j < n) {
        if (!charSet.contains(s.charAt(j))) {
            charSet.add(s.charAt(j));
            j++;
            maxLength = Math.max(maxLength, j - i);
        } else {
            charSet.remove(s.charAt(i));
            i++;
        }
    }
    
    return maxLength;
}
```

### Complexity Analysis
- **Time Complexity:** O(2n) = O(n). In the worst case, each character will be visited twice.
- **Space Complexity:** O(min(n, m)). The set is used to store the unique characters in the window.

## Approach 3: Optimized Sliding Window Using Map

### Intuition
Further optimization can be done by optimizing the move of the start pointer. We can use a hash map to track the last seen index of each character, which allows skipping over sections of the string already known to contain non-unique characters.

### Implementation
```java
public int lengthOfLongestSubstring(String s) {
    int n = s.length();
    int maxLength = 0;
    int i = 0;
    Map<Character, Integer> map = new HashMap<>();
    
    for (int j = 0; j < n; j++) {
        if (map.containsKey(s.charAt(j))) {
            // Update start of window to new position
            i = Math.max(i, map.get(s.charAt(j)) + 1);
        }
        map.put(s.charAt(j), j);
        maxLength = Math.max(maxLength, j - i + 1);
    }
    
    return maxLength;
}
```

### Complexity Analysis
- **Time Complexity:** O(n). Each character in the string is processed at most once.
- **Space Complexity:** O(min(n, m)). HashMap space is used to store last indices of characters.

