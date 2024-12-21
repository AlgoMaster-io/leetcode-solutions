# [Leetcode 2353: Design a Food Rating System](https://leetcode.com/problems/design-a-food-rating-system/)

## Solutions
1. [Basic Approach: Using Arrays](#basic-approach-using-arrays)
2. [Optimized Approach: Using Dictionary and MinHeap](#optimized-approach-using-dictionary-and-minheap)

### Basic Approach: Using Arrays

#### Intuition
The foundational approach is to use simple arrays to store the `food`, `cuisine`, and `ratings` data. This method will involve iterating over arrays to perform the necessary operations like updating ratings and retrieving highest rated food for a given cuisine. While straightforward, this approach is not efficient for large datasets as searching through arrays can be time-consuming.

#### Approach
1. Store the `foods`, `cuisines`, and `ratings` in separate arrays.
2. For updating a food's rating, find the index in the `foods` array and update the corresponding `ratings` value.
3. To find the highest rated food for a cuisine, filter the `cuisines` array for the given cuisine and then search for the maximum rating.

#### Complexity Analysis
- **Time Complexity**: 
  - For `changeRating`: O(n), as we might need to scan the entire list to find the food.
  - For `highestRated`: O(n), as we need to find the maximum rating for a particular cuisine.
- **Space Complexity**: O(n), since we are storing the initial arrays.

```javascript
class FoodRatings {
    constructor(foods, cuisines, ratings) {
        // Store foods, cuisines, and ratings as arrays
        this.foods = foods;
        this.cuisines = cuisines;
        this.ratings = ratings;
    }

    changeRating(food, newRating) {
        const index = this.foods.indexOf(food);
        if (index !== -1) {
            // Update the rating of the specified food
            this.ratings[index] = newRating;
        }
    }

    highestRated(cuisine) {
        let maxRating = -1;
        let highestRatedFood = '';

        // Iterate over all foods to find the highest rated food for the specified cuisine
        for (let i = 0; i < this.foods.length; i++) {
            if (this.cuisines[i] === cuisine && (this.ratings[i] > maxRating || (this.ratings[i] === maxRating && this.foods[i] < highestRatedFood))) {
                maxRating = this.ratings[i];
                highestRatedFood = this.foods[i];
            }
        }
        
        return highestRatedFood;
    }
}
```

### Optimized Approach: Using Dictionary and MinHeap

#### Intuition
The more optimal strategy involves utilizing a dictionary to map foods to their respective ratings and another dictionary to group foods by their cuisine. This is combined with a MinHeap (or priority queue) to efficiently keep track of the highest-rated foods for each cuisine. This approach ensures faster access, update, and retrieval operations.

#### Approach
1. Use a dictionary to map each `food` to both its `cuisine` and `rating`.
2. Use a dictionary where each `cuisine` maps to a priority queue (implemented as a MinHeap) that tracks the foods by their ratings.
3. For `changeRating`, update the rating in the dictionary and adjust the priority queue accordingly.
4. For `highestRated`, simply peek the top element of the respective cuisine's priority queue.

#### Complexity Analysis
- **Time Complexity**: 
  - For `changeRating`: O(log n) due to heap operations.
  - For `highestRated`: O(1) to retrieve the highest rated food.
- **Space Complexity**: O(n), primarily for storing mappings and heaps for each cuisine.

```javascript
const MinHeap = require('min-heap'); // Assume we have a min-heap implementation

class FoodRatings {
    constructor(foods, cuisines, ratings) {
        this.foodMap = new Map();
        this.cuisineMap = new Map();

        // Initialize the mappings
        for (let i = 0; i < foods.length; i++) {
            const food = foods[i];
            const cuisine = cuisines[i];
            const rating = ratings[i];

            // Map food to its rating and cuisine
            this.foodMap.set(food, { cuisine, rating });

            // Add the food to the corresponding cuisine heap
            if (!this.cuisineMap.has(cuisine)) {
                this.cuisineMap.set(cuisine, new MinHeap((a, b) => a.rating - b.rating || a.food.localeCompare(b.food)));
            }
            this.cuisineMap.get(cuisine).push({ food, rating });
        }
    }

    changeRating(food, newRating) {
        if (!this.foodMap.has(food)) return;

        const { cuisine } = this.foodMap.get(food);

        // Update the rating in the map
        this.foodMap.set(food, { ...this.foodMap.get(food), rating: newRating });

        // Update the priority queue
        const heap = this.cuisineMap.get(cuisine);
        heap.push({ food, rating: newRating }); // Pushing could create duplicates, handle this depending on heap operations
    }

    highestRated(cuisine) {
        const heap = this.cuisineMap.get(cuisine);
        let top = heap.peek(); // This assumes peek without removal, or appropriate lazy removal

        // Lazy heap removal: while top of heap isn't updated, remove it
        while (heap.size() && this.foodMap.get(top.food).rating !== top.rating) {
            heap.pop();
            top = heap.peek();
        }

        return top.food;
    }
}
```

Note: An actual implementation would require a custom priority queue (heap) data structure tailored to efficiently support both insertion and lazy removal of outdated food items.

