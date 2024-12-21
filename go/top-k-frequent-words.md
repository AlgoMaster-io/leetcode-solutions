# [Leetcode 692: Top K Frequent Words](https://leetcode.com/problems/top-k-frequent-words/)

## Approaches
1. [Naive Approach using Sorting](#naive-approach-using-sorting)
2. [Using Min-Heap with a Frequency Map](#using-min-heap-with-a-frequency-map)
3. [Optimal Approach using Bucket Sort](#optimal-approach-using-bucket-sort)

### Naive Approach using Sorting

#### Intuition
In this approach, we first count the occurrences of each word using a hashmap (`map[string]int`). Once we have the frequency map, we can convert it into a list, sort it first by the frequency in descending order, and then by lexicographical order in case of ties. Finally, we select the top k elements from the sorted list.

#### Code
```go
package main

import (
	"fmt"
	"sort"
)

func topKFrequent(words []string, k int) []string {
	// Step 1: Build the frequency map
	frequencyMap := make(map[string]int)
	for _, word := range words {
		frequencyMap[word]++
	}

	// Step 2: Convert map to slice of pairs
	type wordFreq struct {
		word string
		freq int
	}
	wordList := []wordFreq{}
	for word, freq := range frequencyMap {
		wordList = append(wordList, wordFreq{word, freq})
	}

	// Step 3: Sort the words based on frequency and lexicographical order
	sort.Slice(wordList, func(i, j int) bool {
		if wordList[i].freq == wordList[j].freq {
			return wordList[i].word < wordList[j].word
		}
		return wordList[i].freq > wordList[j].freq
	})

	// Step 4: Retrieve the top k frequent words
	result := []string{}
	for i := 0; i < k; i++ {
		result = append(result, wordList[i].word)
	}
	return result
}

func main() {
	words := []string{"i", "love", "leetcode", "i", "love", "coding"}
	k := 2
	fmt.Println(topKFrequent(words, k)) // Output: ["i", "love"]
}
```

#### Time Complexity
- Time: O(N log N) where N is the number of unique words, due to sorting.
- Space: O(N) for storing the frequency map and list.

### Using Min-Heap with a Frequency Map

#### Intuition
Instead of sorting all elements, we can use a min-heap to keep track of the top k frequent words. By using a heap, we maintain the top k elements efficiently. This approach helps optimize the sorting process, especially beneficial when the list of unique words is large relative to k.

#### Code
```go
package main

import (
	"container/heap"
	"fmt"
	"sort"
)

type WordFreq struct {
	word string
	freq int
}

type MinHeap []WordFreq

func (h MinHeap) Len() int           { return len(h) }
func (h MinHeap) Less(i, j int) bool { return h[i].freq < h[j].freq || (h[i].freq == h[j].freq && h[i].word > h[j].word) }
func (h MinHeap) Swap(i, j int)      { h[i], h[j] = h[j], h[i] }

func (h *MinHeap) Push(x interface{}) {
	*h = append(*h, x.(WordFreq))
}

func (h *MinHeap) Pop() interface{} {
	old := *h
	n := len(old)
	item := old[n-1]
	*h = old[0 : n-1]
	return item
}

func topKFrequentWithHeap(words []string, k int) []string {
	// Step 1: Build the frequency map
	frequencyMap := make(map[string]int)
	for _, word := range words {
		frequencyMap[word]++
	}

	// Step 2: Use a min heap to maintain the top k frequent elements
	h := &MinHeap{}
	heap.Init(h)

	for word, freq := range frequencyMap {
		heap.Push(h, WordFreq{word, freq})
		if h.Len() > k {
			heap.Pop(h)
		}
	}

	// Step 3: Extract elements from the heap into the result array
	result := make([]string, 0, k)
	for h.Len() > 0 {
		result = append(result, heap.Pop(h).(WordFreq).word)
	}

	// Step 4: We must reverse the result because it should be in descending order
	for i, j := 0, len(result)-1; i < j; i, j = i+1, j-1 {
		result[i], result[j] = result[j], result[i]
	}

	return result
}

func main() {
	words := []string{"i", "love", "leetcode", "i", "love", "coding"}
	k := 2
	fmt.Println(topKFrequentWithHeap(words, k)) // Output: ["i", "love"]
}
```

#### Time Complexity
- Time: O(N log k) due to heap operations where N is the number of unique words.
- Space: O(N) for the frequency map and O(k) for the heap.

### Optimal Approach using Bucket Sort

#### Intuition
In this optimal approach, we use a bucket sort-like technique. We maintain an array (or list of lists) where each index represents the frequency, and each element in the list at that index represents words with that frequency. This way, we eliminate the need for sorting and directly access frequent elements.

#### Code
```go
package main

import (
	"fmt"
	"sort"
)

func topKFrequentBucketSort(words []string, k int) []string {
	// Step 1: Build the frequency map
	frequencyMap := make(map[string]int)
	for _, word := range words {
		frequencyMap[word]++
	}

	// Step 2: Use buckets to sort words by frequency
	maxFreq := 0
	buckets := make(map[int][]string)
	for word, freq := range frequencyMap {
		buckets[freq] = append(buckets[freq], word)
		if freq > maxFreq {
			maxFreq = freq
		}
	}

	// Result array
	result := []string{}

	// Step 3: Collect words from highest frequency to lowest
	for freq := maxFreq; freq > 0 && len(result) < k; freq-- {
		if bucket, found := buckets[freq]; found {
			// Sort words with same frequency lexicographically
			sort.Strings(bucket)
			result = append(result, bucket...)
			if len(result) > k {
				result = result[:k] // Trim to k if we have more than needed
			}
		}
	}
	return result
}

func main() {
	words := []string{"i", "love", "leetcode", "i", "love", "coding"}
	k := 2
	fmt.Println(topKFrequentBucketSort(words, k)) // Output: ["i", "love"]
}
```

#### Time Complexity
- Time: O(N + k log N)
- Space: O(N) for storing the frequency map and bucket list.

This approach is optimal when k is smaller than the total unique word count, providing a balance between time and space complexity.

