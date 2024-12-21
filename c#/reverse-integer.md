# [Leetcode 7: Reverse Integer](https://leetcode.com/problems/reverse-integer/)

## Approaches

- [Approach 1: String Conversion and Reversal](#approach-1-string-conversion-and-reversal)
- [Approach 2: Mathematical Reversal with Overflow Check](#approach-2-mathematical-reversal-with-overflow-check)

## Approach 1: String Conversion and Reversal

### Intuition
The simplest way to reverse an integer is to convert it to a string, reverse the string, and then convert it back to an integer. While this approach is straightforward and easy to implement, it does require additional space for string manipulation and doesn't handle integer overflow.

### Implementation

```csharp
public int Reverse(int x) {
    // Convert the integer to a string
    string str = x.ToString();

    // Check if the number is negative
    bool isNegative = str[0] == '-';
    
    // Reverse the string
    char[] charArray;
    if (isNegative) {
        // Skip the negative sign for the reversal
        charArray = str.Substring(1).ToCharArray();
    } else {
        charArray = str.ToCharArray();
    }
    Array.Reverse(charArray);
    
    // Convert back to integer
    string reversedStr = new string(charArray);
    try {
        int reversedInt = int.Parse(reversedStr);
        return isNegative ? -reversedInt : reversedInt;
    } catch (OverflowException) {
        return 0; // Return 0 in case of overflow
    }
}
```

### Complexity Analysis
- **Time Complexity**: O(n), where n is the number of digits in the integer.
- **Space Complexity**: O(n), due to the storage of string data.

---

## Approach 2: Mathematical Reversal with Overflow Check

### Intuition
We can reverse the integer mathematically by repeatedly extracting the last digit using modulo operation and building the reversed number. To prevent overflow, we carefully check the intermediate reversed number against integer limits before each addition.

### Implementation

```csharp
public int Reverse(int x) {
    int reversed = 0;
    while (x != 0) {
        // Extract the last digit
        int digit = x % 10;
        
        // Check for overflow: before multiplying reversed by 10, ensure it won't overflow
        if (reversed > int.MaxValue / 10 || (reversed == int.MaxValue / 10 && digit > 7)) {
            return 0;
        }
        if (reversed < int.MinValue / 10 || (reversed == int.MinValue / 10 && digit < -8)) {
            return 0;
        }
        
        // Append the last digit to the reversed number
        reversed = reversed * 10 + digit;
        
        // Remove the last digit from the original number
        x /= 10;
    }
    return reversed;
}
```

### Complexity Analysis
- **Time Complexity**: O(n), where n is the number of digits in the integer.
- **Space Complexity**: O(1), because we are using a constant amount of additional space.

The mathematical reversal method is preferred for its efficiency and direct handling of overflow issues.

