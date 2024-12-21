# [LeetCode 1044: Longest Duplicate Substring](https://leetcode.com/problems/longest-duplicate-substring/)

## Solutions

- [Solution 1: Brute Force](#solution-1-brute-force)
- [Solution 2: Using Binary Search and Rabin-Karp Algorithm](#solution-2-using-binary-search-and-rabin-karp-algorithm)

### Solution 1: Brute Force

This solution examines every possible substring and checks if there is any repeating substring. It's highly inefficient but simple to implement and reason about.

1. **Intuition**:
   - We generate all possible substrings of various lengths.
   - For each substring, check if it appears more than once in the original string.
   - Keep track of the longest substrings that have duplicates.

2. **Steps**:
   - Iterate through all possible starting points and lengths of substrings.
   - Use a hash map or set to track substrings that have been seen.
   - If a substring is found in the set again, update the longest substring found.

3. **Code**:
   ```cpp
   #include <unordered_set>
   #include <string>
   using namespace std;

   string longestDupSubstring(string s) {
       string longest = "";
       const int n = s.length();
       for (int len = 1; len < n; ++len) { // Consider all lengths
           unordered_set<string> seen;
           for (int i = 0; i <= n - len; ++i) { // Generate all substrings of this length
               string sub = s.substr(i, len);
               if (seen.count(sub)) { // Check if we have already seen this substring
                   if (sub.length() > longest.length())
                       longest = sub;
               }
               seen.insert(sub);
           }
       }
       return longest;
   }
   ```

4. **Complexity**:
   - **Time Complexity**: O(N^3) for generating all substrings and checking them.
   - **Space Complexity**: O(N^2) due to storage of substrings.

### Solution 2: Using Binary Search and Rabin-Karp Algorithm

This is the optimal solution that utilizes rolling hash and binary search to find the longest duplicate substring.

1. **Intuition**:
   - Use binary search to find the maximum length of substring that occurs more than once.
   - Use the Rabin-Karp algorithm to check if a substring of a given length is a duplicate.

2. **Steps**:
   - Perform binary search on the length of the substring, from 1 to n - 1.
   - At each midpoint length, use rolling hash to find if any duplicate substring of that length exists.
   - If duplicates are found, try searching for a longer length; otherwise, search for a shorter length.

3. **Code**:
   ```cpp
   #include <unordered_set>
   #include <vector>
   #include <string>
   using namespace std;

   class Solution {
   public:
       string longestDupSubstring(string S) {
           int n = S.size();

           // Helper function for binary search
           auto check = [&](int len) {
               long long mod = 1e9 + 7;
               long long base = 256;
               long long hash = 0;
               long long baseL = 1;
               unordered_set<long long> seen;

               // Calculate hash for the first 'len' characters
               for (int i = 0; i < len; ++i) {
                   hash = (hash * base + S[i]) % mod;
                   baseL = (baseL * base) % mod;
               }
               seen.insert(hash);

               // Rolling hash
               for (int start = len; start < n; ++start) {
                   hash = ((hash * base - S[start - len] * baseL % mod + mod) % mod + S[start]) % mod;
                   if (seen.count(hash))
                       return start - len + 1;
                   seen.insert(hash);
               }
               return -1;
           };

           int left = 1, right = n - 1;
           int start = 0, maxLen = 0;
           while (left <= right) {
               int mid = left + (right - left) / 2;
               int idx = check(mid);
               if (idx != -1) {
                   start = idx;
                   maxLen = mid;
                   left = mid + 1;
               } else {
                   right = mid - 1;
               }
           }
           return S.substr(start, maxLen);
       }
   };
   ```

4. **Complexity**:
   - **Time Complexity**: O(N log N) due to binary search and rolling hash.
   - **Space Complexity**: O(N) for hash storage.

