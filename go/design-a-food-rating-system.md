# [Leetcode Problem 2353: Design a Food Rating System](https://leetcode.com/problems/design-a-food-rating-system/)

## Approaches
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Optimized with Hashmaps and Priority Queues](#approach-2-optimized-with-hashmaps-and-priority-queues)

### Approach 1: Brute Force

**Intuition:**

To implement a basic food rating system, we can use simple data structures that maintain a list of foods and their corresponding ratings for each cuisine.

For each query to find the highest-rated food of a specific cuisine, we can simply iterate through the list of foods for that cuisine, checking each one to find the highest rating.

**Detailed Comments:**
1. Maintain a list of foods, their ratings, and their cuisines. 
2. For `highestRated` queries, iterate through the entire list of foods for that cuisine to find the highest-rated one.
3. Updating involves simply changing the rating in our list or array.

**Code:**
```go
type FoodRatings struct {
	foods       []string
	ratings     []int
	cuisines    []string
	foodIndex   map[string]int // To keep the index of food in the array
	cuisineList map[string][]string // To group foods by cuisines
}

func Constructor(foods []string, cuisines []string, ratings []int) FoodRatings {
	foodIndex := make(map[string]int)
	cuisineList := make(map[string][]string)
	for i, food := range foods {
		foodIndex[food] = i
		cuisineList[cuisines[i]] = append(cuisineList[cuisines[i]], food)
	}
	return FoodRatings{foods, ratings, cuisines, foodIndex, cuisineList}
}

func (this *FoodRatings) ChangeRating(food string, newRating int) {
	index := this.foodIndex[food] // Get the index of the food
	this.ratings[index] = newRating // Update its rating
}

func (this *FoodRatings) HighestRated(cuisine string) string {
	bestFood := ""
	highestRating := -1

	// Iterate through the foods in the given cuisine
	for _, food := range this.cuisineList[cuisine] {
		index := this.foodIndex[food]
		if this.ratings[index] > highestRating ||
			(this.ratings[index] == highestRating && food < bestFood) {
			bestFood = food
			highestRating = this.ratings[index]
		}
	}
	return bestFood
}
```

**Complexity Analysis:**
- **Time Complexity:** 
  - `ChangeRating`: O(1)
  - `HighestRated`: O(N) where N is the number of foods in the cuisine.
- **Space Complexity:** O(N), storing all foods, their cuisines, and indexes.

### Approach 2: Optimized with Hashmaps and Priority Queues

**Intuition:**

To optimize, especially for frequent `HighestRated` queries, we can use heaps (priority queues) to manage the foods by their ratings and names. This will allow us to efficiently retrieve the highest-rated food quickly.

**Detailed Comments:**
1. Use a combination of hashmaps to store foods, ratings, and cuisines.
2. Use a max-heap (priority queue) for each cuisine storing (negative rating, food name) to easily get the highest rated food.
3. Changing a rating involves adjusting the heap since Go's heap does not support update, we may need to rebuild the heap.

**Code:**
```go
import (
	"container/heap"
)

type FoodRatings struct {
	foodRating  map[string]int
	foodCuisine map[string]string
	cuisines    map[string]*FoodHeap 
}

type FoodHeap []string

func Constructor(foods []string, cuisines []string, ratings []int) FoodRatings {
	foodRating := make(map[string]int)
	foodCuisine := make(map[string]string)
	cuisinesMap := make(map[string]*FoodHeap)
	for i, food := range foods {
		foodRating[food] = ratings[i]
		foodCuisine[food] = cuisines[i]
		if cuisinesMap[cuisines[i]] == nil {
			cuisinesMap[cuisines[i]] = &FoodHeap{}
		}
		heap.Push(cuisinesMap[cuisines[i]], food)
	}

	return FoodRatings{foodRating, foodCuisine, cuisinesMap}
}

func (this *FoodRatings) ChangeRating(food string, newRating int) {
	cuisine := this.foodCuisine[food]
	this.foodRating[food] = newRating
	heapify(this.cuisines[cuisine], this.foodRating)
}

func (this *FoodRatings) HighestRated(cuisine string) string {
	fh := this.cuisines[cuisine]
	for {
		food := (*fh)[0]
		if this.foodRating[food] == -(*fh)[0] {
			return food
		}
		heap.Pop(fh)
	}
}

// Implement heap.Interface for FoodHeap
func (fh FoodHeap) Len() int { return len(fh) }
func (fh FoodHeap) Less(i, j int) bool {
	// Compare by rating, then by name if ratings are equal
	ri, rj := fh[i].foodRating[fh[i]], fh[j].foodRating[fh[j]]
	if ri == rj {
		return fh[i] < fh[j]
	}
	return ri > rj
}

func (fh FoodHeap) Swap(i, j int) {
	fh[i], fh[j] = fh[j], fh[i]
}

func (fh *FoodHeap) Push(x interface{}) {
	*fh = append(*fh, x.(string))
}

func (fh *FoodHeap) Pop() interface{} {
	old := *fh
	n := len(old)
	x := old[n-1]
	*fh = old[0 : n-1]
	return x
}

func heapify(h *FoodHeap, ratings map[string]int) {
	newHeap := &FoodHeap{}
	for _, food := range *h {
		heap.Push(newHeap, food)
	}
	*h = *newHeap
}
```

**Complexity Analysis:**
- **Time Complexity:**
  - `ChangeRating`: O(log N) where N is the number of foods in the cuisine.
  - `HighestRated`: O(1) amortized due to lazy deletions and heap maintenance.
- **Space Complexity:** O(N) where N is the number of foods, for storing heaps for each cuisine.

