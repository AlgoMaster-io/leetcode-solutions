## [Leetcode 1525: Number of Good Ways to Split a String](https://leetcode.com/problems/number-of-good-ways-to-split-a-string/)

### Approaches
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Using Two Pass with Prefix Arrays](#approach-2-using-two-pass-with-prefix-arrays)
- [Approach 3: Optimal Sliding Window](#approach-3-optimal-sliding-window)

---

### Approach 1: Brute Force

**Intuition**:  
In the brute force approach, we will try all possible split positions in the string to see if they meet the criteria of having an equal number of unique characters on both sides. This involves scanning the entire string segment before and after each split to count the unique characters.

**Steps**:
1. For each possible split (`i`) from `1` to `n-1`:
   - Use a set to count the unique characters in the left part.
   - Use another set to count the unique characters in the right part.
   - Compare the counts, and if they are equal, increment a counter for valid splits.

```cpp
int numSplits(std::string s) {
    int n = s.size();
    int goodSplits = 0;
    for (int i = 1; i < n; ++i) {
        std::set<char> leftSet(s.begin(), s.begin() + i);
        std::set<char> rightSet(s.begin() + i, s.end());
        if (leftSet.size() == rightSet.size()) {
            ++goodSplits;
        }
    }
    return goodSplits;
}
```

**Time Complexity**: O(n^2), where n is the length of the string, due to the nested use of set operations.

**Space Complexity**: O(n), maximum space used by two sets at a time.

---

### Approach 2: Using Two Pass with Prefix Arrays

**Intuition**:  
A two-pass approach allows us to precompute the number of unique characters from the start to each position and from the end to each position. This makes it easy to compute the difference in unique counts without recalculating them for each potential split.

**Steps**:
1. Precompute prefixes:
   - Use a frequency map to track unique characters from the left.
   - Use an array to store the count of unique characters at every index from left.
2. Repeat for the right side.
3. Check for splits by comparing precomputed values.

```cpp
int numSplits(std::string s) {
    int n = s.size();
    if (n == 0) return 0;

    std::vector<int> leftUnique(n, 0), rightUnique(n, 0);
    std::unordered_map<char, int> freqMap;
    int uniqueChars = 0;

    // Calculate unique characters from left
    for (int i = 0; i < n; ++i) {
        if (++freqMap[s[i]] == 1) {
            ++uniqueChars;
        }
        leftUnique[i] = uniqueChars;
    }

    // Reset for right calculation
    freqMap.clear();
    uniqueChars = 0;

    // Calculate unique characters from right
    for (int i = n - 1; i >= 0; --i) {
        if (++freqMap[s[i]] == 1) {
            ++uniqueChars;
        }
        rightUnique[i] = uniqueChars;
    }

    int goodSplits = 0;
    // Compare left and right unique counts
    for (int i = 0; i < n - 1; ++i) {
        if (leftUnique[i] == rightUnique[i + 1]) {
            ++goodSplits;
        }
    }
    
    return goodSplits;
}
```

**Time Complexity**: O(n), due to two linear passes over the string.

**Space Complexity**: O(n), for storing the unique counts for both left and right arrays.

---

### Approach 3: Optimal Sliding Window

**Intuition**:  
Optimize the current two-pass solution by using a single pass with a sliding window. Tracking the balance of unique characters efficiently can lead to better performance by reducing space usage.

**Steps**:
1. Incrementally update counters for unique characters on both the left and right.
2. Adjust the right counter as you move the split index.
3. Use two sets to keep track of the unique elements seen so far from left and right and update as needed.

```cpp
int numSplits(std::string s) {
    int n = s.size();
    std::vector<int> leftUnique(26, 0), rightUnique(26, 0);
    int uniqueLeft = 0, uniqueRight = 0;

    // Initialize right
    for (char c : s) {
        if (++rightUnique[c - 'a'] == 1) {
            ++uniqueRight;
        }
    }

    int goodSplits = 0;
    for (int i = 0; i < n - 1; ++i) {
        char currentChar = s[i];
        
        // Add to left's count
        if (++leftUnique[currentChar - 'a'] == 1) {
            ++uniqueLeft;
        }
        
        // Remove from right's count
        if (--rightUnique[currentChar - 'a'] == 0) {
            --uniqueRight;
        }
        
        // Compare the counts of unique chars
        if (uniqueLeft == uniqueRight) {
            ++goodSplits;
        }
    }

    return goodSplits;
}
```

**Time Complexity**: O(n), linear scan maintaining counts with constant-time operations.

**Space Complexity**: O(1), uses fixed-size data structures for counting (26 for English alphabet).

---

