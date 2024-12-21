## [Leetcode 151: Reverse Words in a String](https://leetcode.com/problems/reverse-words-in-a-string/)

### Approaches:
- [Approach 1: Using Built-in Functions](#approach-1-using-built-in-functions)
- [Approach 2: Two Pointers with Deque](#approach-2-two-pointers-with-deque)
- [Approach 3: In-place Replacement using Two Pointers](#approach-3-in-place-replacement-using-two-pointers)

### Approach 1: Using Built-in Functions

**Intuition:**

This approach leverages built-in Java functions to split the string by spaces, remove any empty elements, and reverse the resulting word list. This is straightforward but relies heavily on library functions.

**Steps:**
1. Split the input string by spaces.
2. Filter out any empty strings resulting from extra spaces.
3. Reverse the list and join the words into a single string with space separation.

```java
class Solution {
    public String reverseWords(String s) {
        // Step 1: Split the input string on spaces
        String[] words = s.trim().split("\\s+");
        
        // Step 2: Use StringBuilder to construct the result efficiently
        StringBuilder reversed = new StringBuilder();
        for (int i = words.length - 1; i >= 0; i--) {
            reversed.append(words[i]);
            if (i != 0) {
                reversed.append(" ");
            }
        }
        
        return reversed.toString();
    }
}
```

**Time Complexity:** O(N), where N is the length of the input string, due to splitting and joining operations.

**Space Complexity:** O(N), used by split and the StringBuilder.

---

### Approach 2: Two Pointers with Deque

**Intuition:**

Using a two-pointer approach along with a deque (double-ended queue) can help manage space efficiently and avoid unnecessary memory allocation beyond what is required to store words. This technique manually traverses the string and accumulates words in reverse order.

**Steps:**
1. Traverse the string from end to start using two pointers to find words.
2. Use a deque to efficiently manage the words that should appear first in the result.
3. Append words to the front of the deque and join them at the end.

```java
import java.util.Deque;
import java.util.LinkedList;

class Solution {
    public String reverseWords(String s) {
        int left = 0, right = s.length() - 1;
        // Step 1: Trim leading and trailing spaces
        while (left <= right && s.charAt(left) == ' ') left++;
        while (left <= right && s.charAt(right) == ' ') right--;

        Deque<String> d = new LinkedList<>();
        StringBuilder word = new StringBuilder();

        // Step 2: Go from the last character to the first
        while (left <= right) {
            char c = s.charAt(left);

            // If it's a space and a word has been completely read
            if ((word.length() != 0) && (c == ' ')) {
                d.offerFirst(word.toString());
                word.setLength(0); // Reset the word
            } else if (c != ' ') {
                word.append(c); // Add non-space characters to the current word
            }
            left++;
        }

        // Add the last word
        d.offerFirst(word.toString());

        // Return the joined words in the order they were added to deque
        return String.join(" ", d);
    }
}
```

**Time Complexity:** O(N), where N is the length of the input string, as we traverse each character once.

**Space Complexity:** O(N), used by the deque.

---

### Approach 3: In-place Replacement using Two Pointers

**Intuition:**

This method reverses the entire string first, then reverses each word in place. This technique is more space efficient because the reversal occurs directly within the input string resorted to character arrays.

**Steps:**
1. Reverse the entire character array.
2. Reverse each word within the array.
3. Clean up any extra spaces resulting from the reversal process.

```java
class Solution {
    public String reverseWords(String s) {
        // Convert string to char array for in-place modifications
        char[] str = s.toCharArray();

        // Step 1: Reverse entire string
        reverse(str, 0, str.length - 1);

        // Step 2: Reverse each word
        reverseWords(str);

        // Step 3: Clean up spaces and return the cleaned string
        return cleanSpaces(str);
    }

    private void reverse(char[] str, int left, int right) {
        while (left < right) {
            char temp = str[left];
            str[left++] = str[right];
            str[right--] = temp;
        }
    }

    private void reverseWords(char[] s) {
        int n = s.length;
        int start = 0;
        for (int end = 0; end < n; end++) {
            // Find the end of the current word
            if (s[end] == ' ') {
                reverse(s, start, end - 1);
                start = end + 1; // Move to the start of the next word
            }
        }
        // Reverse the last word
        reverse(s, start, n - 1);
    }

    private String cleanSpaces(char[] str) {
        int n = str.length;
        int i = 0, j = 0;

        while (j < n) {
            // Skip spaces
            while (j < n && str[j] == ' ') j++;
            // Copy non-space characters
            while (j < n && str[j] != ' ') str[i++] = str[j++];
            // Skip spaces to reach the next word, add only one space if there's a next word
            while (j < n && str[j] == ' ') j++;
            if (j < n) str[i++] = ' ';
        }

        return new String(str, 0, i);
    }
}
```

**Time Complexity:** O(N), where N is the length of the string, due to multiple in-place passes.

**Space Complexity:** O(1), as modifications are performed in-place without extra space allocations outside of the input array.

