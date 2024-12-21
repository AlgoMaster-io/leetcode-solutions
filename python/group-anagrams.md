# [Leetcode 49: Group Anagrams](https://leetcode.com/problems/group-anagrams/)

## Approaches
- [Approach 1: Using Sorting and Hash Map](#approach-1-using-sorting-and-hash-map)
- [Approach 2: Using Counting and Hash Map](#approach-2-using-counting-and-hash-map)

### Approach 1: Using Sorting and Hash Map

#### Intuition:
The idea is to use the fact that all anagrams, when sorted, result in the same string. Thus, by sorting each string in the input list and using the sorted string as the key in a hash map, we can group all anagrams together.

#### Steps:
1. Initialize an empty dictionary to store the sorted string as the key and the list of anagrams as the value.
2. Iterate over each string in the input list:
   - Sort the string and convert it back to a tuple which can be used as a key in a dictionary.
   - Append the original string to the list of strings stored under the sorted tuple in the dictionary.
3. The values of the dictionary will now represent the grouped anagrams. Return these values.

#### Code:
```python
def groupAnagrams(strs):
    anagrams = {}
    
    for s in strs:
        # Sort the string and use it as a hashable key
        sorted_str = tuple(sorted(s))
        
        # Append the original string to the correct group in the dictionary
        if sorted_str not in anagrams:
            anagrams[sorted_str] = []
        
        anagrams[sorted_str].append(s)
    
    # Return all the grouped anagrams
    return list(anagrams.values())
```

#### Time Complexity:
- Sorting each string takes \(O(m \log m)\), where \(m\) is the length of the string.
- Thus, the overall time complexity is \(O(n \times m \log m)\), where \(n\) is the number of strings.

#### Space Complexity:
- The space complexity is \(O(n \times m)\) to store the sorted keys and the grouped anagrams.

### Approach 2: Using Counting and Hash Map

#### Intuition:
Instead of sorting, we can count the frequency of each character in the string. Anagrams will have the same character frequency for all characters from 'a' to 'z'.

#### Steps:
1. Initialize a dictionary to store the character count tuple as a key.
2. For each string, create a count of 26 zeros (for each letter in the alphabet).
3. Increment the count for each character in the string.
4. Use this count tuple as a key in the dictionary to group the anagrams.
5. The dictionary values will then be the grouped anagrams.

#### Code:
```python
def groupAnagrams(strs):
    anagrams = {}
    
    for s in strs:
        # Create a count of 26 zeros (one for each letter)
        count = [0] * 26
        
        # Count the occurrences of each character
        for char in s:
            count[ord(char) - ord('a')] += 1
        
        # Convert the list to a tuple so it can be a dictionary key
        count_tuple = tuple(count)
        
        # Append the original string to the correct group in the dictionary
        if count_tuple not in anagrams:
            anagrams[count_tuple] = []
        
        anagrams[count_tuple].append(s)
    
    # Return all the grouped anagrams
    return list(anagrams.values())
```

#### Time Complexity:
- Counting the characters takes \(O(m)\), where \(m\) is the length of the string.
- Thus, the overall time complexity is \(O(n \times m)\).

#### Space Complexity:
- The space complexity remains \(O(n \times m)\) due to the storage of the frequency count keys and grouped anagrams.

