# [792. Number of Matching Subsequences](https://leetcode.com/problems/number-of-matching-subsequences/)

## Table of Contents

- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Sorting and Two Pointers](#approach-2-sorting-and-two-pointers)
- [Approach 3: Using Buckets and Queues](#approach-3-using-buckets-and-queues)

### Approach 1: Brute Force

**Intuition:**

The simplest approach to this problem is to iterate over each word and check if it is a subsequence of the string `S`. A subsequence means that the characters of the word appear in the same order in `S`, but not necessarily consecutively.

**Steps:**

1. For each word, use two pointers to check if it is a subsequence of `S`.
2. The first pointer will iterate over `S`, and the second pointer will iterate over the word.
3. If all characters of the word are found in `S` in order, increment the count.

```go
func numMatchingSubseq(S string, words []string) int {
    matchCount := 0

    for _, word := range words {
        if isSubsequence(S, word) {
            matchCount++
        }
    }

    return matchCount
}

func isSubsequence(S, word string) bool {
    i, j := 0, 0
    for i < len(S) && j < len(word) {
        if S[i] == word[j] {
            j++
        }
        i++
    }
    return j == len(word)
}
```

**Time Complexity:** O(n * m), where `n` is the number of words and `m` is the average length of the words.

**Space Complexity:** O(1)

### Approach 2: Sorting and Two Pointers

**Intuition:**

We can enhance the brute force approach by using sorting and two pointers. We sort each word's characters and use two pointers to check if the sorted word characters match sequentially in the sorted string `S`.

**Steps:**

1. Sort the string `S`.
2. For each word, sort its characters and use two pointers to compare if the sorted word is a subsequence of the sorted `S`.

```go
import "sort"

func numMatchingSubseq(S string, words []string) int {
    matchCount := 0
    sSorted := []rune(S)
    sort.Slice(sSorted, func(i, j int) bool {
        return sSorted[i] < sSorted[j]
    })

    for _, word := range words {
        if isSubsequenceSorted(string(sSorted), word) {
            matchCount++
        }
    }

    return matchCount
}

func isSubsequenceSorted(sSorted, word string) bool {
    wSorted := []rune(word)
    sort.Slice(wSorted, func(i, j int) bool {
        return wSorted[i] < wSorted[j]
    })

    i, j := 0, 0
    for i < len(sSorted) && j < len(wSorted) {
        if sSorted[i] == wSorted[j] {
            j++
        }
        i++
    }
    return j == len(wSorted)
}
```

**Time Complexity:** O(n * m * log(m) + m * log(n)), due to sorting the string `S` and each word.

**Space Complexity:** O(m + n)

### Approach 3: Using Buckets and Queues

**Intuition:**

Instead of sorting, we can use a more efficient technique by leveraging character buckets with an array of queues. This minimizes redundant checks and efficiently manages subsequences.

**Steps:**

1. Create buckets for each character in the alphabet; each bucket is a queue.
2. Iterate through `S` and, for each character, advance matching subsequence pointers present in appropriate queues.
3. For each completed subsequence, increment the count.

```go
func numMatchingSubseq(S string, words []string) int {
    matchCount := 0
    buckets := make([][]*string, 26)

    // Initialize buckets
    for i := 0; i < 26; i++ {
        buckets[i] = []*string{}
    }

    // Populate the buckets with words
    for i := 0; i < len(words); i++ {
        firstChar := words[i][0] - 'a'
        buckets[firstChar] = append(buckets[firstChar], &words[i])
    }

    // Iterate over characters in S
    for _, c := range S {
        bucket := buckets[c-'a']
        buckets[c-'a'] = []*string{} // Clear current bucket

        for _, wordPtr := range bucket {
            word := *wordPtr
            if len(word) == 1 { // This is the last character
                matchCount++
            } else {
                nextChar := word[1] - 'a'
                buckets[nextChar] = append(buckets[nextChar], new(string))
                *buckets[nextChar][len(buckets[nextChar])-1] = word[1:]
            }
        }
    }

    return matchCount
}
```

**Time Complexity:** O(S + sum(len(word) for word in words)), efficiently processing each character and subsequence only as required.

**Space Complexity:** O(m), where `m` is the total length of all words in input.

This approach utilizes an efficient use of space with a keen focus on time optimization regarding iterating over and processing character matches.

