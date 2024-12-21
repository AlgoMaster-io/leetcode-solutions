# [Leetcode 496: Next Greater Element I](https://leetcode.com/problems/next-greater-element-i/)

## Approaches:
- [Brute Force](#brute-force)
- [Stack with HashMap](#stack-with-hashmap)

---

### Brute Force

#### Intuition:
In the brute force solution, for each element in `nums1`, you would traverse `nums2` to find the element and then continue searching for the next greater element. If no such element exists, we return `-1`.

#### Steps:
1. Initialize an empty array `result` to store the answer for each element in `nums1`.
2. For each number `n` in `nums1`:
   - Find `n` in `nums2`.
   - From the position of `n`, traverse through the rest of the `nums2` to find the next greater element.
   - If a greater element is found, append it to `result`.
   - If no greater element is found, append `-1` to `result`.
3. Return the `result` vector.

#### Time and Space Complexity:
- **Time Complexity:** O(n * m), where n is the length of `nums1` and m is the length of `nums2`. This is because for each element in `nums1`, we may traverse the entire `nums2`.
- **Space Complexity:** O(1) if we ignore the space used by the output.

```go
func nextGreaterElement(nums1 []int, nums2 []int) []int {
    result := make([]int, len(nums1))
    
    // Iterate over each element in nums1
    for i, n := range nums1 {
        found := false
        greaterFound := false
        // Traverse nums2 to find n and next greater element
        for _, num := range nums2 {
            if num == n {
                found = true
            }
            if found && num > n {
                result[i] = num
                greaterFound = true
                break
            }
        }
        if !greaterFound {
            result[i] = -1
        }
    }
    return result
}
```

---

### Stack with HashMap

#### Intuition:
We can use a stack to efficiently determine the next greater element for each number in `nums2`. By iterating through `nums2`, we can maintain a decreasing sequence of numbers in the stack. As soon as we find a number that's greater than the stack's top, we've found the next greater element for the numbers in the stack.

#### Steps:
1. Initialize an empty stack `stack` and a hashmap `nextGreaterMap`.
2. Iterate through each element in `nums2`:
   - While the stack is not empty and the current number is greater than the stack’s top, pop from the stack and record the current number as the next greater element for the popped number in `nextGreaterMap`.
   - Push the current number onto the stack.
3. Initialize the result array based on `nums1`. For each number in `nums1`, look up its next greater element using `nextGreaterMap`.
4. If there is no entry in `nextGreaterMap` for a number, append `-1` in the result.
5. Return the result.

#### Time and Space Complexity:
- **Time Complexity:** O(n + m), where n is the length of `nums1` and m is the length of `nums2`. Each element is processed at most twice.
- **Space Complexity:** O(m), due to the storage of map entries.

```go
func nextGreaterElement(nums1 []int, nums2 []int) []int {
    stack := []int{}
    nextGreaterMap := make(map[int]int)
    
    // Iterate over nums2 to find the next greater element for each number
    for _, num := range nums2 {
        // Determine the next greater element for the top of stack
        for len(stack) > 0 && num > stack[len(stack)-1] {
            top := stack[len(stack)-1]
            stack = stack[:len(stack)-1]
            nextGreaterMap[top] = num
        }
        stack = append(stack, num)
    }
    
    // Prepare the result for nums1 using the calculated nextGreaterMap
    result := make([]int, len(nums1))
    for i, n := range nums1 {
        if val, exists := nextGreaterMap[n]; exists {
            result[i] = val
        } else {
            result[i] = -1
        }
    }
    return result
}
```

Each of these solutions explores different techniques to solve the problem, providing variations in time and space efficiency. The stack with hashmap approach demonstrates how extra space can be used to optimize time complexity significantly over a brute force approach.

