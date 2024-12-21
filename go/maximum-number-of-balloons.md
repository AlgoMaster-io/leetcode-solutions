# [Leetcode 1189: Maximum Number of Balloons](https://leetcode.com/problems/maximum-number-of-balloons/)

In this problem, we're required to determine the maximum number of times we can form the word "balloon" using the letters from a given string text.

## Approaches
1. [Frequency Counting and Simple Calculation](#frequency-counting-and-simple-calculation)
2. [Optimized Frequency Counting with Early Stopping](#optimized-frequency-counting-with-early-stopping)

---

## Approach 1: Frequency Counting and Simple Calculation

### Intuition
The basic idea is to first count the frequency of each character in the given string `text`. After that, for the word "balloon", we calculate how many times we can fully extract it from these frequencies. Note that 'l' and 'o' occur twice in "balloon," so we need to adjust for that.

### Steps
1. Create a frequency map (hash map) to count occurrences of each character in `text`.
2. For each character in "balloon", calculate the possible number of times it can contribute to the formation of the word.
3. The limiting character (i.e., the one with the fewest complete contributions) dictates the maximum number of "balloon" words we can form.
4. Return the minimum of these contributions.

### Complexity Analysis
- **Time Complexity:** O(n), where n is the length of the text, as each character is processed once.
- **Space Complexity:** O(1), since we only need a constant amount of extra space for the frequency map.

### Code

```go
func maxNumberOfBalloons(text string) int {
    // Step 1: Count the occurrences of each character in the text.
    freq := make(map[rune]int)
    for _, char := range text {
        freq[char]++
    }

    // Step 2: Determine the number of times we can form "balloon".
    // "balloon" -> {b:1, a:1, l:2, o:2, n:1}
    balloonCount := map[rune]int{'b': 1, 'a': 1, 'l': 2, 'o': 2, 'n': 1}
    minBalloonCount := len(text) // Start with a large number

    for char, count := range balloonCount {
        // Calculate the number of times we can supply this character
        if freq[char] < count {
            // If any required character is not present, we cannot form a single "balloon"
            return 0
        }
        totalCharContributions := freq[char] / count
        if totalCharContributions < minBalloonCount {
            minBalloonCount = totalCharContributions
        }
    }

    return minBalloonCount
}
```

## Approach 2: Optimized Frequency Counting with Early Stopping

### Intuition
The previous approach can be slightly optimized by checking if any character falls below its required frequency earlier, allowing us to potentially stop calculations prematurely.

### Steps
1. Similar to the first approach, create a frequency map for the given text.
2. As you iterate over the word "balloon", as soon as you find a character whose count does not permit forming the word at least once, return 0.
3. Otherwise, keep track of the minimal possible contribution from each character.

### Complexity Analysis
- **Time Complexity:** O(n), where n is the length of the text.
- **Space Complexity:** O(1), as the extra space used for the frequency map is limited.

### Code

```go
func maxNumberOfBalloonsOptimized(text string) int {
    freq := make(map[rune]int)
    for _, char := range text {
        freq[char]++
    }

    balloonCount := map[rune]int{'b': 1, 'a': 1, 'l': 2, 'o': 2, 'n': 1}
    minBalloonCount := len(text)

    for char, count := range balloonCount {
        if freq[char] < count {
            return 0
        }
        currentContribution := freq[char] / count
        if currentContribution < minBalloonCount {
            minBalloonCount = currentContribution
        }
    }

    return minBalloonCount
}
```

Both approaches arrive at a solution utilizing a frequency map to efficiently count characters and compute the minimum occurrences required to form the word "balloon", resulting in an optimal and straightforward solution.

