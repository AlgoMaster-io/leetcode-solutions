# [Leetcode 659: Split Array into Consecutive Subsequences](https://leetcode.com/problems/split-array-into-consecutive-subsequences/)

## Approaches
1. [Basic Approach: Priority Queues and HashMap](#approach-1)
2. [Optimal Approach: Greedy with HashMap](#approach-2)

---

## Approach 1: Basic Approach: Priority Queues and HashMap

### Intuition
The problem requires us to find subsequences where each is at least length 3 and numbers are consecutive. A naive approach is to utilize a priority queue to keep track of subsequences ends. For each number, try to extend a subsequence ending just before this number. If not possible, start a new subsequence.

### Steps
1. Use a dictionary to map each number to a priority queue, where the priority queue keeps track of all subsequences ending with that number and their length.
2. Traverse through each number in the array and determine if it can extend an existing subsequence or start a new one.
3. If a number `num` can extend one subsequence ending with `num-1`, pop the subsequence from the queue, increment its length, and push onto the queue for `num`.
4. If `num` is starting a new subsequence, push it with an initial length of 1 onto the `num` queue.
5. Finally, verify all priority queues to ensure all subsequences are at least length 3.

### Time Complexity
- **O(n log k)**: Where `n` is the length of the array, and `k` is the number of unique subsequences.
  
### Space Complexity
- **O(n)**: Storing the subsequences and their lengths.

```csharp
public bool IsPossible(int[] nums) {
    if (nums == null || nums.Length < 3) return false;

    // Dictionary to store the end of subsequences with their lengths
    var subsequencesEnd = new Dictionary<int, PriorityQueue<int, int>>();

    foreach (int num in nums) {
        if (!subsequencesEnd.ContainsKey(num)) {
            subsequencesEnd[num] = new PriorityQueue<int, int>(new ComparisonComparer<int>((a, b) => a.CompareTo(b)));
        }
        
        // Check if there is a subsequence ending with num-1
        if (subsequencesEnd.ContainsKey(num - 1) && subsequencesEnd[num - 1].Count > 0) {
            int previousLength = subsequencesEnd[num - 1].Dequeue();
            subsequencesEnd[num].Enqueue(previousLength + 1, previousLength + 1);
        } else {
            // Start a new subsequence
            subsequencesEnd[num].Enqueue(1, 1);
        }
    }

    // Validate if all subsequences are at least length 3
    foreach (var kvp in subsequencesEnd) {
        var endsQueue = kvp.Value;
        while (endsQueue.Count > 0) {
            if (endsQueue.Dequeue() < 3)
                return false;
        }
    }
    
    return true;
}

// Custom comparer to allow PriorityQueue for .NET 5+
class ComparisonComparer<T> : IComparer<T> {
    private readonly Comparison<T> _comparison;
    public ComparisonComparer(Comparison<T> comparison) {
        _comparison = comparison ?? throw new ArgumentNullException(nameof(comparison));
    }
    public int Compare(T x, T y) {
        return _comparison(x, y);
    }
}
```

---

## Approach 2: Optimal Approach: Greedy with HashMap

### Intuition
The optimal solution leverages greedy strategy: For each number, decide if it can extend an existing subsequence or begin a new sequence. Focus on optimizing subsequence length by always trying to append a number to existing subsequences before starting new ones.

### Steps
1. Utilize two dictionaries: `freq` to keep track of occurrences of each number, and `appendFreq` to track if a subsequence ending could extend using this number.
2. Iterate through the list, decreasing `freq` count for each number being considered.
3. Try to append `num` to an existing subsequence that ends just before it (look up in `appendFreq`).
4. If impossible, check if `num` can be the start of a new subsequence by confirming the presence of `num+1` and `num+2`.
5. Adjust `appendFreq` for future potential subsequences appropriately.
6. If neither extension nor creation is possible, return false, as the condition fails.

### Time Complexity
- **O(n)**: Each number visited once, adjusting frequency counts.

### Space Complexity
- **O(n)**: Two dictionaries to store frequency and extension possibilities.

```csharp
public bool IsPossible(int[] nums) {
    if (nums == null || nums.Length < 3) return false;

    // Frequency dictionary for the numbers
    var freq = new Dictionary<int, int>();
    // Dictionary to track potentially formable subsequences ending at a particular number
    var appendFreq = new Dictionary<int, int>();

    // Populate frequency dictionary
    foreach (int num in nums) {
        if (!freq.ContainsKey(num))
            freq[num] = 0;
        freq[num]++;
    }

    // Try building subsequences
    foreach (int num in nums) {
        if (freq[num] == 0) continue; // Already used number
        
        // Check if can append to an existing subsequence
        if (appendFreq.ContainsKey(num) && appendFreq[num] > 0) {
            appendFreq[num]--;
            if (!appendFreq.ContainsKey(num + 1))
                appendFreq[num + 1] = 0;
            appendFreq[num + 1]++;
        } 
        // Or create a new subsequence
        else if (freq.ContainsKey(num + 1) && freq[num + 1] > 0 && 
                 freq.ContainsKey(num + 2) && freq[num + 2] > 0) {
            freq[num + 1]--;
            freq[num + 2]--;
            if (!appendFreq.ContainsKey(num + 3))
                appendFreq[num + 3] = 0;
            appendFreq[num + 3]++;
        } 
        else {
            return false; // Neither option is possible
        }

        // Reduce frequency of current number
        freq[num]--;
    }

    return true;
}
```

This greedy solution efficiently checks and creates subsequences by leveraging hashmaps to track necessary details, achieving optimal performance for large arrays.

