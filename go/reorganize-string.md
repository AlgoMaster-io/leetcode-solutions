# [Leetcode 767: Reorganize String](https://leetcode.com/problems/reorganize-string/)

## Solutions
- [Approach 1: Count Characters and Use a Priority Queue (Heap)](#approach-1)
- [Approach 2: Greedy Placement](#approach-2)

### Approach 1: Count Characters and Use a Priority Queue (Heap)

#### Intuition
The idea is to use a max heap (priority queue) to always place the character with the highest frequency that isn't the same as the last placed character. By always placing the highest frequency character available, we stand a better chance of avoiding consecutive duplicates.

#### Steps:
1. Count frequencies of each character into a map.
2. Push all characters with their frequencies into a max heap.
3. Pop from this heap and append characters into the result string, ensuring we don't place two identical characters consecutively.

#### Algorithm
- If a character's frequency is more than half the length of the string plus one, it's impossible to form a properly reorganized string.
- Use a priority queue to handle the selection of the next character to append.
- Keep track of the last appended character to avoid placing the same character consecutively.

```go
import (
  "container/heap"
  "strings"
)

// Pair holds the character and its negative frequency for max-heap simulation.
type Pair struct {
  char     rune
  negativeFreq int
}

// PriorityQueue implements heap.Interface for Pair.
type PriorityQueue []Pair

func (pq PriorityQueue) Len() int { return len(pq) }
func (pq PriorityQueue) Less(i, j int) bool { return pq[i].negativeFreq < pq[j].negativeFreq }
func (pq PriorityQueue) Swap(i, j int) { pq[i], pq[j] = pq[j], pq[i] }

func (pq *PriorityQueue) Push(x interface{}) {
  *pq = append(*pq, x.(Pair))
}

func (pq *PriorityQueue) Pop() interface{} {
  old := *pq
  n := len(old)
  item := old[n-1]
  *pq = old[:n-1]
  return item
}

// reorganizeString returns a reorganized string without adjacent repeating characters.
func reorganizeString(s string) string {
  // Count frequencies of each character
  freq := make(map[rune]int)
  for _, ch := range s {
    freq[ch]++
  }

  // Build a max heap based on the negative frequency
  pq := &PriorityQueue{}
  heap.Init(pq)
  
  for ch, count := range freq {
    if count > (len(s)+1)/2 {
      return "" // impossible to reorganize
    }
    heap.Push(pq, Pair{ch, -count})
  }

  var resultBuilder strings.Builder
  prevPair := Pair{0, 0}

  // Greedily append characters
  for pq.Len() > 0 {
    current := heap.Pop(pq).(Pair)
    resultBuilder.WriteRune(current.char)

    if prevPair.negativeFreq < 0 {
      heap.Push(pq, prevPair)
    }

    current.negativeFreq++
    prevPair = current
  }

  return resultBuilder.String()
}
```

#### Time Complexity
- O(n log k), where n is the length of the string, and k is the number of unique characters (to keep the heap of size k).

#### Space Complexity
- O(k), the space for the heap.

---

### Approach 2: Greedy Placement

#### Intuition
An alternative approach exploits the observation around balancing out the most frequent character first by filling the even indices and then the odd indices in the result array.

#### Steps:
1. Count frequencies of characters.
2. Determine the most frequent character and ensure its frequency does not exceed half the string length plus one.
3. Fill even indices with the most frequent character until exhausted, then fill odd indices with the remaining characters.

#### Algorithm
- Similar to a two-pass filling of a placeholder with even and odd indices.
- Seamless allocation of characters into specified empty slots ensuring no adjacent duplicates.

```go
func reorganizeString(s string) string {
  n := len(s)
  
  // Count frequencies
  freq := make([]int, 26)
  for _, ch := range s {
    freq[ch-'a']++
  }
  
  // Determine the max frequency character
  maxFreq, maxChar := 0, 0
  for i := 0; i < 26; i++ {
    if freq[i] > maxFreq {
      maxFreq = freq[i]
      maxChar = i
    }
  }
  
  // If it's impossible to reorganize
  if maxFreq > (n+1)/2 {
    return ""
  }
  
  // Initialize result array
  result := make([]byte, n)
  idx := 0
  
  // Start filling in the most frequent character first
  for ; freq[maxChar] > 0; freq[maxChar]-- {
    result[idx] = byte(maxChar + 'a')
    idx += 2  // Fill even positions first
  }
  
  // Fill the remaining characters
  for i := 0; i < 26; i++ {
    for freq[i] > 0 {
      if idx >= n {
        idx = 1  // Start filling odd positions if even spots are filled
      }
      result[idx] = byte(i + 'a')
      idx += 2
      freq[i]--
    }
  }
  
  return string(result)
}
```

#### Time Complexity
- O(n) for iterating and filling the result.

#### Space Complexity
- O(1) extra space, aside from input/output, as it uses a fixed size array to count character frequencies.

