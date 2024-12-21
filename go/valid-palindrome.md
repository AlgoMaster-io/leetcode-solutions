# [Leetcode 125: Valid Palindrome](https://leetcode.com/problems/valid-palindrome/)

## Approaches
1. [Two Pointers with Character Filtering](#approach1)
2. [Optimized Two Pointers without Extra Storage](#approach2)

---

## Approach 1: Two Pointers with Character Filtering

### Intuition
The core idea of this approach is to filter the string to remove any non-alphanumeric characters and convert it to lowercase. Once we have the filtered string, we use two pointers, one starting at the beginning (`left`) and another at the end (`right`) of the string. We move the pointers towards the center, comparing characters at each step. If all comparisons are equal, the string is a palindrome.

### Solution
```go
func isPalindrome(s string) bool {
    // Function to check if a character is alphanumeric
    isAlphanumeric := func(c byte) bool {
        return (c >= 'a' && c <= 'z') || (c >= 'A' && c <= 'Z') || (c >= '0' && c <= '9')
    }
    
    // Filter out non-alphanumeric characters and convert to lowercase
    filtered := []byte{}
    for i := 0; i < len(s); i++ {
        if isAlphanumeric(s[i]) {
            filtered = append(filtered, toLower(s[i]))
        }
    }
    
    // Initialize two pointers
    left, right := 0, len(filtered) - 1
    for left < right {
        // Compare characters at the two pointers
        if filtered[left] != filtered[right] {
            return false
        }
        left++
        right--
    }
    return true
}

// Helper function to convert a byte to lowercase
func toLower(c byte) byte {
    if c >= 'A' && c <= 'Z' {
        return c + ('a' - 'A')
    }
    return c
}
```

### Time Complexity
- O(n), where n is the length of the input string. We iterate over the string once to filter it and once more to check for the palindrome.

### Space Complexity
- O(n), due to the additional storage used for the filtered string.

---

## Approach 2: Optimized Two Pointers without Extra Storage

### Intuition
This approach eliminates the need for storing a filtered version of the string. We can perform the filtering process and palindrome checking simultaneously using two pointers (`left` and `right`) iterating from the start and end of the string. Both pointers move inward, skipping over any non-alphanumeric characters and comparing the filtered characters directly.

### Solution
```go
func isPalindrome(s string) bool {
    left, right := 0, len(s) - 1
    
    // While the pointers have not crossed each other
    for left < right {
        // Move left pointer until an alphanumeric character is encountered
        for left < right && !isAlphanumeric(s[left]) {
            left++
        }
        // Move right pointer until an alphanumeric character is encountered
        for left < right && !isAlphanumeric(s[right]) {
            right--
        }
        
        // Compare characters at the current pointers, ignoring case
        if toLower(s[left]) != toLower(s[right]) {
            return false
        }
        left++
        right--
    }
    return true
}

// Helper function remains unchanged
func isAlphanumeric(c byte) bool {
    return (c >= 'a' && c <= 'z') || (c >= 'A' && c <= 'Z') || (c >= '0' && c <= '9')
}

func toLower(c byte) byte {
    if c >= 'A' && c <= 'Z' {
        return c + ('a' - 'A')
    }
    return c
}
```

### Time Complexity
- O(n), similar to the first approach, we iterate through the string once.

### Space Complexity
- O(1), as no additional storage beyond a few integer variables is used.
   
By using two pointers and moving them from both ends towards the center while skipping non-alphanumeric characters, we efficiently determine whether the input string is a palindrome.

