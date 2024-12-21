# [Leetcode 451: Sort Characters By Frequency](https://leetcode.com/problems/sort-characters-by-frequency/)

## Approaches
1. [Frequency Counting with Sorting](#approach-1-frequency-counting-with-sorting)
2. [Bucket Sort](#approach-2-bucket-sort)

---

## Approach 1: Frequency Counting with Sorting

**Intuition:**

The simplest approach to this problem is to count the frequency of each character and then sort the characters based on their frequencies. We can utilize a `Dictionary` to store character frequencies, and then sort these characters. The major steps include:

1. Traverse the input string to count the frequency of each character using a `Dictionary`.
2. Convert the dictionary to a list and sort based on character frequency in descending order.
3. Build the result string based on this sorted list.

This approach is straightforward but may not be the most optimal due to sorting overhead.

**Time Complexity:** `O(n log n)` where `n` is the length of the string (due to sorting).
**Space Complexity:** `O(n)`

```csharp
public string FrequencySort(string s) {
    if (string.IsNullOrEmpty(s)) return s;
    
    // Step 1: Count frequency of each character
    Dictionary<char, int> charFrequency = new Dictionary<char, int>();
    foreach (char c in s) {
        if (charFrequency.ContainsKey(c))
            charFrequency[c]++;
        else
            charFrequency[c] = 1;
    }
    
    // Step 2: Convert the dictionary to a list of KeyValuePairs and sort
    List<KeyValuePair<char, int>> charList = charFrequency.ToList();
    charList.Sort((a, b) => b.Value.CompareTo(a.Value));
    
    // Step 3: Build the result string
    StringBuilder result = new StringBuilder();
    foreach (KeyValuePair<char, int> pair in charList) {
        result.Append(new string(pair.Key, pair.Value));
    }
    
    return result.ToString();
}
```

---

## Approach 2: Bucket Sort

**Intuition:**

The second and more optimal approach involves using a bucket sort-like technique. Here are the main steps:

1. Calculate the frequency of each character using a dictionary similar to the first approach.
2. Create buckets for storing characters by frequency. Each bucket index corresponds to a possible frequency.
3. Populate these buckets with corresponding characters.
4. Build the output string by concatenating strings formed in these buckets in descending order of frequency.

This approach eliminates the need to sort the frequency list, leading to potential performance improvements for very large input sizes.

**Time Complexity:** `O(n)` where `n` is the length of the string.
**Space Complexity:** `O(n)`

```csharp
public string FrequencySort(string s) {
    if (string.IsNullOrEmpty(s)) return s;
    
    // Step 1: Count frequency of each character
    Dictionary<char, int> charFrequency = new Dictionary<char, int>();
    foreach (char c in s) {
        if (charFrequency.ContainsKey(c))
            charFrequency[c]++;
        else
            charFrequency[c] = 1;
    }
    
    // Step 2: Create buckets for each frequency
    // The max frequency is bounded by the length of the string
    List<char>[] buckets = new List<char>[s.Length + 1];
    foreach (var pair in charFrequency) {
        int frequency = pair.Value;
        if (buckets[frequency] == null) {
            buckets[frequency] = new List<char>();
        }
        buckets[frequency].Add(pair.Key);
    }
    
    // Step 3: Build output string
    StringBuilder result = new StringBuilder();
    for (int i = buckets.Length - 1; i >= 0; i--) {
        if (buckets[i] != null) {
            foreach (char c in buckets[i]) {
                result.Append(new string(c, i));
            }
        }
    }
    
    return result.ToString();
}
```

By using the bucket sort approach, we effectively deal with the problem in linear time, which is a notable improvement over sorting-based methods for cases where the frequency count does not equate directly with the length of the string. This demonstrates an optimal solution making the solution efficient and scalable.

