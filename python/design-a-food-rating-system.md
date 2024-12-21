# [Leetcode 2353: Design a Food Rating System](https://leetcode.com/problems/design-a-food-rating-system/)

### Navigation
- [Approach 1: Using Dictionaries and Sorting](#approach-1-using-dictionaries-and-sorting)
- [Approach 2: Using Heaps for Efficient Sorting](#approach-2-using-heaps-for-efficient-sorting)

## Problem Summary
The problem requires the design of a food rating system where you can perform these operations:
1. Initialize the system with lists of foods, their cuisines, and ratings.
2. Update the rating of a specific food.
3. Retrieve the highest-rated food for a specific cuisine.

## Approach 1: Using Dictionaries and Sorting

### Intuition:
We can utilize dictionaries to map foods to their ratings and cuisines to a list of foods. For each query to find the highest-rated food for a cuisine, we can sort the list of foods for that cuisine by their ratings. Though simple, this approach may become inefficient when frequently searching for the highest-rated food, as sorting takes additional time.

### Implementation:

```python
class FoodRatings:

    def __init__(self, foods, cuisines, ratings):
        # Initialize a dictionary to store food to its rating
        self.food_to_rating = {}
        # Initialize a dictionary to store cuisine to a list of foods
        self.cuisine_to_foods = {}
        
        # Iterate over the list of foods to initialize the dictionaries
        for food, cuisine, rating in zip(foods, cuisines, ratings):
            self.food_to_rating[food] = (rating, cuisine)
            if cuisine not in self.cuisine_to_foods:
                self.cuisine_to_foods[cuisine] = []
            self.cuisine_to_foods[cuisine].append(food)

    def changeRating(self, food, newRating):
        # Change food's rating
        rating, cuisine = self.food_to_rating[food]
        self.food_to_rating[food] = (newRating, cuisine)

    def highestRated(self, cuisine):
        # Sort foods by rating and return the highest one
        foods = self.cuisine_to_foods[cuisine]
        foods.sort(key=lambda food: (-self.food_to_rating[food][0], food))
        return foods[0]  # Return the food with highest rating

```

### Time Complexity:
- `__init__`: O(N) where N is the number of foods.
- `changeRating`: O(1) constant time for updating a dictionary.
- `highestRated`: O(M log M), where M is the number of foods in the queried cuisine (due to sorting).

### Space Complexity:
- O(N), for storing ratings and cuisines in dictionaries.

## Approach 2: Using Heaps for Efficient Sorting

### Intuition:
To optimize the approach, especially for frequent queries on highest-rated foods, we can utilize heaps. A heap will naturally keep the highest (or lowest) element at the top, ensuring efficient retrieval without the need for full sorting each time.

### Implementation:

```python
from heapq import heappush, heappop, heappushpop

class FoodRatings:

    def __init__(self, foods, cuisines, ratings):
        self.food_to_rating = {}
        self.cuisine_to_heap = {}

        # Initialize data from input
        for food, cuisine, rating in zip(foods, cuisines, ratings):
            self.food_to_rating[food] = (rating, cuisine)
            if cuisine not in self.cuisine_to_heap:
                self.cuisine_to_heap[cuisine] = []
            # Use negative rating for max-heap simulation
            heappush(self.cuisine_to_heap[cuisine], (-rating, food))

    def changeRating(self, food, newRating):
        rating, cuisine = self.food_to_rating[food]
        self.food_to_rating[food] = (newRating, cuisine)
        # Push the new rating into the heap
        heappush(self.cuisine_to_heap[cuisine], (-newRating, food))

    def highestRated(self, cuisine):
        # Pop from heap until a valid entry is found
        while self.cuisine_to_heap[cuisine]:
            rating, food = self.cuisine_to_heap[cuisine][0]
            if -rating == self.food_to_rating[food][0]:
                return food
            heappop(self.cuisine_to_heap[cuisine])

```

### Time Complexity:
- `__init__`: O(N log N) for heap operations.
- `changeRating`: O(log M) where M is entries in the heap (due to heap push).
- `highestRated`: O(log M) amortized, due to heap properties.

### Space Complexity:
- O(N), for the heap storage and dictionary.

Both approaches provide a mechanism to handle food rating systems although the heap-based approach generally offers better performance in terms of frequent retrieval operations.

