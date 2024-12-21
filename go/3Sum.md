# [Leetcode 15: 3Sum](https://leetcode.com/problems/3sum/)

## Approaches

1. [Brute Force](#brute-force)
2. [Two Pointers](#two-pointers)

## Brute Force

### Intuition

The simplest approach to solve the problem is to use three nested loops to check each combination of elements to find triples that sum up to zero. This approach directly checks every possible combination, which can become time-consuming for larger arrays.

### Approach

- Use three nested loops to pick each combination of three different elements from the array.
- For each combination, check if the sum of the trio is zero.
- If it is, add the triplet to the result list, ensuring to keep it unique.

### Pseudocode

```go
func threeSum(nums []int) [][]int {
    n := len(nums)
    result := [][]int{}
    for i := 0; i < n; i++ {
        for j := i + 1; j < n; j++ {
            for k := j + 1; k < n; k++ {
                if nums[i]+nums[j]+nums[k] == 0 {
                    // Sort the triplet to handle uniqueness of results
                    triplet := []int{nums[i], nums[j], nums[k]}
                    sort.Ints(triplet)
                    if !contains(result, triplet) {
                        result = append(result, triplet)
                    }
                }
            }
        }
    }
    return result
}

func contains(triplets [][]int, triplet []int) bool {
    for _, t := range triplets {
        if reflect.DeepEqual(t, triplet) {
            return true
        }
    }
    return false
}
```

### Complexity Analysis

- **Time Complexity**: \(O(n^3 \cdot log(m))\), where \(n\) is the length of `nums`, and \(m\) is the number of unique triplets.
- **Space Complexity**: \(O(m)\), the space used to store unique sets of triplets.

## Two Pointers

### Intuition

To optimize our approach, we can use sorting and the two-pointer technique. After sorting the array, we can fix one element and use a two-pointer technique to find two others such that the sum is zero. This reduces the problem to a two-sum problem.

### Approach

- Sort the array to make it easier to avoid duplicates and use the two-pointer technique.
- Iterate through each element, fixing it as the first element of the potential triplet.
- Use two pointers from the elements immediately after the fixed element to the end of the array.
- Adjust the pointers based on the sum of the triplet to either increase or decrease the sum until the triplet sums to zero.

### Detailed Steps

1. **Sort the Array**: First, sort the array to facilitate using pointers to find pairs.
2. **Iterate through elements**: Fix one element and then use two pointers to find the other two elements.
3. **Two-pointer Technique**: 
   - Initialize two pointers, one (left) starting just after the fixed element and another (right) at the end of the array.
   - Calculate the sum of `nums[fixed]`, `nums[left]`, and `nums[right]`.
   - If the sum is zero, add the triplet to the result (if it's unique).
   - If the sum is less than zero, move the left pointer to the right to increase the sum.
   - If the sum is greater than zero, move the right pointer to the left to decrease the sum.
4. **Skip Duplicates**: To ensure uniqueness, skip elements that are the same as previous ones after finding each triplet.

### Code

```go
func threeSum(nums []int) [][]int {
    sort.Ints(nums)
    result := [][]int{}
    n := len(nums)
    
    for i := 0; i < n-2; i++ {
        // Skip duplicate elements
        if i > 0 && nums[i] == nums[i-1] {
            continue
        }
        
        left := i + 1
        right := n - 1
        
        while left < right {
            sum := nums[i] + nums[left] + nums[right]
            
            if sum == 0 {
                result = append(result, []int{nums[i], nums[left], nums[right]})
                // Skip duplicates at the left pointer
                for left < right && nums[left] == nums[left+1] {
                    left++
                }
                // Skip duplicates at the right pointer
                for left < right && nums[right] == nums[right-1] {
                    right--
                }
                left++
                right--
            } else if sum < 0 {
                left++
            } else {
                right--
            }
        }
    }
    
    return result
}
```

### Complexity Analysis

- **Time Complexity**: \(O(n^2)\), as we iterate through the list and for each element potentially traversing again using the two-pointer approach.
- **Space Complexity**: \(O(m)\), for storing the result, where `m` is the number of unique triplets. Sorting takes \(O(1)\) space since it's done in-place.

