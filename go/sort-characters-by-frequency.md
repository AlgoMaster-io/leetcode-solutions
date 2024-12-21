# [Leetcode 451: Sort Characters By Frequency](https://leetcode.com/problems/sort-characters-by-frequency/)

## Approaches:
- [Approach 1: Frequency Mapping with Sorting](#approach-1-frequency-mapping-with-sorting)
- [Approach 2: Frequency Mapping with Bucket Sort (Optimal)](#approach-2-frequency-mapping-with-bucket-sort-optimal)

## Approach 1: Frequency Mapping with Sorting

### Intuition:
In this approach, the idea is to first count the frequency of each character in the given string. Once we have the frequencies, we can sort the characters based on these frequencies in descending order. To achieve this, we can utilize a hashmap to store the frequency of each character and then sort the entries by frequency.

### Steps:
1. Use a hashmap (map) to count the frequency of each character in the input string.
2. Convert the hashmap entries into a slice of characters.
3. Sort this slice based on the frequency of the characters in descending order.
4. Construct the resulting string using the sorted order of characters repeated by their frequencies.

### Code:
```go
func frequencySort(s string) string {
	// Step 1: Build frequency map
	freqMap := make(map[rune]int)
	for _, char := range s {
		freqMap[char]++
	}

	// Convert map keys into a slice
	characters := []rune{}
	for char := range freqMap {
		characters = append(characters, char)
	}

	// Custom sort slice based on frequencies in descending order
	sort.Slice(characters, func(i, j int) bool {
		return freqMap[characters[i]] > freqMap[characters[j]]
	})

	// Construct result string based on sorted characters
	result := []rune{}
	for _, char := range characters {
		for i := 0; i < freqMap[char]; i++ {
			result = append(result, char)
		}
	}

	return string(result)
}
```

### Complexity:
- **Time complexity**: O(n + k log k) where `n` is the length of the string, and `k` is the number of unique characters.
- **Space complexity**: O(n) for storing the frequency map and the result.

## Approach 2: Frequency Mapping with Bucket Sort (Optimal)

### Intuition:
In this optimal approach, instead of sorting the characters based on their frequencies, we use a bucket sort-like method. We can utilize an array of buckets where the index represents the frequency of characters and each bucket stores a list of characters with that frequency. This avoids the need for explicit sorting and results in improved efficiency.

### Steps:
1. Use a hashmap to count the frequency of each character in the input string.
2. Create a slice of buckets where the index represents the frequency and each bucket contains characters with that frequency.
3. Fill the buckets based on character frequencies.
4. Traverse the buckets from highest frequency to lowest, and build the result string.

### Code:
```go
func frequencySortBucket(s string) string {
	// Step 1: Build frequency map
	freqMap := make(map[rune]int)
	for _, char := range s {
		freqMap[char]++
	}

	// Step 2: Create buckets according to frequencies
	maxFreq := len(s)
	buckets := make([][]rune, maxFreq+1)

	for char, freq := range freqMap {
		buckets[freq] = append(buckets[freq], char)
	}

	// Step 3: Build result from buckets
	result := make([]rune, 0, maxFreq)
	for freq := maxFreq; freq > 0; freq-- {
		for _, char := range buckets[freq] {
			for i := 0; i < freq; i++ {
				result = append(result, char)
			}
		}
	}

	return string(result)
}
```

### Complexity:
- **Time complexity**: O(n) where `n` is the length of the string as both frequency counting and bucket filling are linear operations.
- **Space complexity**: O(n) for the frequency map, the buckets, and the result string.

Both approaches solve the problem effectively, but the bucket sort approach is more optimal and avoids the sorting overhead.

