# [Leetcode 214: Shortest Palindrome](https://leetcode.com/problems/shortest-palindrome/)

## Approaches

- [Approach 1: Brute Force Checking](#approach-1)
- [Approach 2: KMP Algorithm and String Manipulation](#approach-2)

---

## Approach 1: Brute Force Checking

### Intuition

The basic idea is to find the largest prefix of the given string that is a palindrome. We can then prepend the reverse of the remaining suffix to make the original string a palindrome. Given that we're checking the prefix, we repeatedly test each substring starting from the whole string down to smaller prefixes.

### Steps

1. Attempt to locate the longest palindromic prefix by checking every prefix of the string.
2. Reverse check from the longest prefix to shortest to see if it's a palindrome.
3. Insert the mirrored leftover part of the string in front to form the shortest palindrome. 

### Time Complexity

- **Worst-case time complexity:** O(n²), because for each possible prefix, we might need to check if it is a palindrome.

### Space Complexity

- **Space complexity:** O(n), due to the use of additional space for storing the reversed substring.

### Code

```cpp
class Solution {
public:
    string shortestPalindrome(string s) {
        int n = s.length();
        // Start with the entire string, then shorten the prefix
        for (int i = n; i >= 0; i--) {
            // Check if the substring from 0 to i is a palindrome
            if (isPalindrome(s, 0, i - 1)) {
                // Take the remaining suffix, reverse it and prepend it to s
                string suffix = s.substr(i);
                reverse(suffix.begin(), suffix.end());
                return suffix + s;
            }
        }
        return ""; // Base case, shouldn't reach here
    }
    
    bool isPalindrome(const string& str, int left, int right) {
        while (left < right) {
            if (str[left] != str[right]) return false;
            left++;
            right--;
        }
        return true;
    }
};
```

---

## Approach 2: KMP Algorithm and String Manipulation

### Intuition

To further optimize the checking of palindromic prefix, we can use the KMP pattern matching algorithm. The key idea is to concatenate the string with its reverse and use KMP to preprocess the combined string to find the longest palindromic prefix efficiently.

### Steps

1. Concatenate the original string `s` with its reverse separated by a non-matching character to avoid overlap, such as `s + '#' + reverse(s)`.
2. Use the KMP table (partial match table) on this new string to determine the longest palindromic prefix length.
3. Use this length to determine the shortest palindrome by prepending the counterpart suffix in reverse.

### Time Complexity

- **Time complexity:** O(n), where n is the length of s. This is because the KMP preprocessing takes O(n) time.

### Space Complexity

- **Space complexity:** O(n), due to the KMP table and string manipulation.

### Code

```cpp
class Solution {
public:
    string shortestPalindrome(string s) {
        string rev_s = s;
        reverse(rev_s.begin(), rev_s.end());
        string temp = s + "#" + rev_s; // Use a special character to avoid overlap
        
        // Build KMP table
        vector<int> kmp(temp.size(), 0);
        for (int i = 1; i < temp.size(); ++i) {
            int j = kmp[i - 1];
            while (j > 0 && temp[i] != temp[j]) {
                j = kmp[j - 1];
            }
            if (temp[i] == temp[j]) {
                j++;
            }
            kmp[i] = j;
        }
        
        // The length of the longest palindromic prefix
        int palLen = kmp[temp.size() - 1];
        
        // Add the prefix to make the string palindrome
        return rev_s.substr(0, s.size() - palLen) + s;
    }
};
```

This method efficiently reduces the complexity of finding the longest palindromic prefix using properties of the KMP algorithm.

