# [767. Reorganize String](https://leetcode.com/problems/reorganize-string/)

## Approach 1: Max Heap

### Solution
go
```go
// Time Complexity: O(n log k), where k is the number of unique characters
// Space Complexity: O(n + k)
package main

import (
    "container/heap"
    "strings"
)

func reorganizeString(s string) string {
    // Frequency map for characters
    charCounts := make([]int, 26)
    for _, c := range s {
        charCounts[c-'a']++
    }

    // Priority queue to keep the most frequent character at the top
    maxHeap := &IntHeap{}
    heap.Init(maxHeap)
    for i, count := range charCounts {
        if count > 0 {
            heap.Push(maxHeap, [2]int{i, count})
        }
    }

    var result strings.Builder
    prev := [2]int{-1, 0} // Previously used character and its count

    for maxHeap.Len() > 0 {
        current := heap.Pop(maxHeap).([2]int)
        result.WriteByte(byte(current[0] + 'a')) // Append current character
        current[1]-- // Decrease its count

        // Re-add the previous character if it's still valid
        if prev[1] > 0 {
            heap.Push(maxHeap, prev)
        }

        // Update the previous character
        prev = current
    }

    // If the result length is not the same as the input, return ""
    if result.Len() != len(s) {
        return ""
    }
    return result.String()
}

type IntHeap [][2]int

func (h IntHeap) Len() int { return len(h) }
func (h IntHeap) Less(i, j int) bool { return h[i][1] > h[j][1] }
func (h IntHeap) Swap(i, j int) { h[i], h[j] = h[j], h[i] }
func (h *IntHeap) Push(x interface{}) { *h = append(*h, x.([2]int)) }
func (h *IntHeap) Pop() interface{} {
    old := *h
    n := len(old)
    x := old[n-1]
    *h = old[0 : n-1]
    return x
}
```

## Approach 2: Greedy with Frequency Array

### Solution
go
```go
// Time Complexity: O(n)
// Space Complexity: O(1) (assuming a fixed alphabet size)
package main

func reorganizeString(s string) string {
    charCounts := make([]int, 26)
    maxCount := 0
    var maxChar byte

    // Count frequencies and find the most frequent character
    for i := 0; i < len(s); i++ {
        charCounts[s[i]-'a']++
        if charCounts[s[i]-'a'] > maxCount {
            maxCount = charCounts[s[i]-'a']
            maxChar = s[i]
        }
    }

    // If the most frequent character cannot fit, return ""
    if maxCount > (len(s)+1)/2 {
        return ""
    }

    result := make([]byte, len(s))
    index := 0

    // Place the most frequent character at even indices
    for charCounts[maxChar-'a'] > 0 {
        result[index] = maxChar
        index += 2
        charCounts[maxChar-'a']--
    }

    // Place the remaining characters
    for i := 0; i < 26; i++ {
        for charCounts[i] > 0 {
            if index >= len(result) {
                index = 1 // Start filling odd indices
            }
            result[index] = byte(i + 'a')
            index += 2
            charCounts[i]--
        }
    }

    return string(result)
}
```

