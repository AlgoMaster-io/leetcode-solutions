# [Leetcode 1512: Number of Good Pairs](https://leetcode.com/problems/number-of-good-pairs/)

## Solutions

- [Brute Force Approach](#brute-force-approach)
- [Optimized Using Hashmap](#optimized-using-hashmap)

### Brute Force Approach

**Intuition**:  
The brute force approach is straightforward as it directly follows from the problem statement. We need to find all pairs `(i, j)` such that `nums[i] == nums[j]` and `i < j`. This can be done by using two nested loops - for each element in the list, check against every subsequent element if they form a good pair.

**Steps**:
1. Initialize a counter `count` to 0.
2. Loop over each element starting from index `i`.
3. For each element, loop over subsequent elements starting from index `i+1`.
4. If the current element at `i` matches with the current element at `j`, increment the `count`.

```go
func numIdenticalPairs(nums []int) int {
    count := 0
    for i := 0; i < len(nums); i++ { // pick the first element
        for j := i + 1; j < len(nums); j++ { // compare with subsequent elements
            if nums[i] == nums[j] { // check if they form a good pair
                count++
            }
        }
    }
    return count
}
```

- **Time Complexity**: O(n^2), where n is the number of elements in the list, due to the two nested loops.
- **Space Complexity**: O(1), as we are only using a constant amount of extra space.

### Optimized Using Hashmap

**Intuition**:  
The brute force method can be optimized by using a hashmap (or dictionary) to track the frequency of each number encountered. For `each number`, if it is seen again, every previous identical number forms a pair with it.

**Steps**:
1. Use a hashmap to store the frequency of each number.
2. Traverse through the list and populate the hashmap.
3. For each number in the list, add its current count in the hashmap to the overall `count`.
4. Increment the count of the number in the hashmap.

```go
func numIdenticalPairs(nums []int) int {
    numCount := make(map[int]int) // map to store frequency of numbers
    count := 0
    
    for _, num := range nums {
        count += numCount[num] // add the number of good pairs before increment
        numCount[num]++ // update the frequency of the current number
    }
    
    return count
}
```

- **Time Complexity**: O(n), where n is the number of elements in the list, as we traverse the list only once.
- **Space Complexity**: O(n), in the worst case, we store each number in the hashmap.

The hashmap approach significantly reduces the time complexity compared to the brute force method by efficiently counting pairs using the frequency of elements.

