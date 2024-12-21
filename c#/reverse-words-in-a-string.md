# [Leetcode 151: Reverse Words in a String](https://leetcode.com/problems/reverse-words-in-a-string/)

## Approaches:
- [Approach 1: Using Built-in String Methods](#approach-1-using-built-in-string-methods)
- [Approach 2: Splitting and Manual Reversal](#approach-2-splitting-and-manual-reversal)

---

### Approach 1: Using Built-in String Methods

**Intuition:**

The idea is to leverage C#'s built-in methods for string manipulation. First, we can split the string into words based on spaces, filter out any empty entries, and then reverse the order of these words. Finally, we'll join them back into a single string with a single space separating each word.

```csharp
public string ReverseWords(string s) {
    // Split the sentence into words
    var words = s.Split(' ');
    
    // Filter out empty spaces that resulted in empty array entries
    var filteredWords = words.Where(w => !string.IsNullOrWhiteSpace(w)).ToArray();
    
    // Reverse the array of words
    Array.Reverse(filteredWords);
    
    // Join the words back into a single string with spaces in between
    return string.Join(" ", filteredWords);
}
```

**Detailed Comments:**
1. `Split(' ')`: Splits the input string `s` into an array of words using a space as the separator.
2. `Where(...)`: Filters out any strings from the array that are null, empty, or whitespace.
3. `Array.Reverse(...)`: Reverses the array in place.
4. `string.Join(" ", ...)`: Joins the filtered, reversed array into a single string with spaces.

**Time Complexity:** O(N), where N is the length of the input string. This complexity arises mainly from iterating through the string to split, reverse and join it back together.

**Space Complexity:** O(N), as we store the resultant words in an additional array.

---

### Approach 2: Splitting and Manual Reversal

**Intuition:**

In this approach, we'll manually process the input string to reverse the words while avoiding empty spaces directly. This will help us reduce any unnecessary iterations or space usage from built-in methods.

```csharp
public string ReverseWords(string s) {
    // Use a list to track words as we find them
    List<string> words = new List<string>();
    int n = s.Length;
    int i = 0;
    
    // Iterate through the string
    while (i < n) {
        if (s[i] == ' ') {
            i++;
            continue;
        }
        
        // Start of a new word
        int start = i;
        
        // Continue until the end of the word
        while (i < n && s[i] != ' ') {
            i++;
        }
        
        // Extract the word and add to list in reverse order
        words.Add(s.Substring(start, i - start));
    }
    
    // Reverse the list of words and join them with a space
    words.Reverse();
    return string.Join(" ", words);
}
```

**Detailed Comments:**
1. Traverse through the string with an index variable `i`.
2. Skip over spaces until we find the start of a word.
3. Capture the start index of the word, and keep iterating until the end of the word.
4. Extract the substring (word) and add it to a list.
5. Once all words are captured, reverse the list and join into a single string separated by space.

**Time Complexity:** O(N), where N is the length of the input string. The primary operations are iterating through the entire string once and reversing the list of words.

**Space Complexity:** O(N), as we need to store a list of words which in the worst case could hold all characters of the input.

---

This concludes the solutions for reversing words in a string. Both methods provide a balance between readability and efficiency, with particular strengths per the use case requirement.

