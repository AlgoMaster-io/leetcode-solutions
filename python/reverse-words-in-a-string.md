## Problem Link
[Leetcode 151: Reverse Words in a String](https://leetcode.com/problems/reverse-words-in-a-string/)

### Table of Contents
- [Approach 1: Built-in Split and Reverse](#approach-1-built-in-split-and-reverse)
- [Approach 2: In-place Reversal (Optimal)](#approach-2-in-place-reversal-optimal)

---

### Approach 1: Built-in Split and Reverse

**Intuition**:  
The straightforward way to tackle this problem is by leveraging Python's built-in string methods. We can:
1. Split the input string on spaces to get a list of words.
2. Reverse the list of words.
3. Join the reversed list back into a single string with spaces in between.

This method is simple and easy to implement, but it's not the most efficient in terms of both time and space complexity.

```python
def reverseWords(s: str) -> str:
    # Step 1: Use split() to divide the string into words
    words = s.split()
    
    # Step 2: Reverse the list of words
    reversed_words = words[::-1]
    
    # Step 3: Join the words back into a string with a single space
    return ' '.join(reversed_words)
```

**Time Complexity**: O(N), where N is the length of the input string. The reason is that split, reverse, and join operations each take linear time in terms of the number of characters.

**Space Complexity**: O(N), as we are storing the split words in a list and creating a new string to return.

---

### Approach 2: In-place Reversal (Optimal)

**Intuition**:  
The most optimal in terms of space is to perform the reversal in place. This method requires more careful manipulation as we need to deal directly with the character array. The steps are as follows:
1. Trim the string to remove leading and trailing spaces.
2. Convert the string to a list of characters for easier manipulation.
3. Reverse the entire list of characters.
4. Reverse each word in the reversed list to restore the word order.
5. Join the words with a single space to form the final output.

```python
def reverseWords(s: str) -> str:
    # Helper function to reverse a portion of the list
    def reverse_list(chars, start, end):
        while start < end:
            chars[start], chars[end] = chars[end], chars[start]
            start, end = start + 1, end - 1

    # Step 1: Trim the input string (Python's split does it for us)
    char_list = list(s.strip())

    # Step 2-3: Reverse the entire list
    reverse_list(char_list, 0, len(char_list) - 1)

    # Step 4: Reverse each word in the reversed list
    n = len(char_list)
    start = 0
    while start < n:
        while start < n and char_list[start] == ' ':
            start += 1
        end = start
        while end < n and char_list[end] != ' ':
            end += 1
        reverse_list(char_list, start, end - 1)
        start = end

    # Step 5: Join the characters back to a string, removing redundant spaces
    return ' '.join(''.join(char_list).split())

```

**Time Complexity**: O(N), as the entire list of characters is traversed a constant number of times. 

**Space Complexity**: O(1), if the input is mutable. Otherwise, O(N) for the list of characters since strings are immutable in Python.

Both approaches address the core of the problem, but the second approach is more memory efficient, making it suitable for very large strings.

