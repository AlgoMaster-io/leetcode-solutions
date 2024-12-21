# [Leetcode 2353: Design a Food Rating System](https://leetcode.com/problems/design-a-food-rating-system/)

## Approaches
- [Approach 1: Brute Force using Sorting](#approach-1)
- [Approach 2: Optimal using Hash Maps and Sorting with Custom Data Structures](#approach-2)

## Approach 1: Brute Force using Sorting

### Intuition
The problem involves creating a system to manage food items and their ratings, allowing us to update ratings and retrieve the highest-rated food in each cuisine. A straightforward approach is to maintain a list of food items with their details: cuisine and rating. For any query regarding the highest-rated food, we can sort the list each time a query is made, based on cuisine and rating.

### Steps
1. Maintain a `foodList` array of objects where each object has `food`, `cuisine`, and `rating`.
2. For updating a food's rating, find the food in the array and modify its rating.
3. To get the highest-rated food from a cuisine, filter the array for the specified cuisine and sort it in descending order based on ratings. Return the food name at the top of the list.

### Code
```typescript
class FoodRatings {
  private foodList: { food: string; cuisine: string; rating: number }[];

  constructor(foods: string[], cuisines: string[], ratings: number[]) {
    this.foodList = foods.map((food, i) => ({
      food,
      cuisine: cuisines[i],
      rating: ratings[i],
    }));
  }

  changeRating(food: string, newRating: number): void {
    const foodItem = this.foodList.find((item) => item.food === food);
    if (foodItem) {
      foodItem.rating = newRating; // Update the rating
    }
  }

  highestRated(cuisine: string): string {
    const candidates = this.foodList
      .filter((item) => item.cuisine === cuisine) // Filter by cuisine
      .sort((a, b) => b.rating - a.rating || a.food.localeCompare(b.food)); // Sort by rating then by food name lexicographically

    return candidates[0].food; // Return the top-rated food
  }
}
```

### Complexity
- **Time Complexity**: 
  - `changeRating()`: O(1) on average due to array search.
  - `highestRated()`: O(n log n) due to sorting.
- **Space Complexity**: O(n) for maintaining the list of foods.

---

## Approach 2: Optimal using Hash Maps and Sorting with Custom Data Structures

### Intuition
For better performance, especially with large datasets, we can optimize by using Hash Maps that allow constant time complexity operations for updates and retrievals. We'll use:
1. A map (`foodToDetails`) to track ratings and cuisines for each food.
2. A map (`cuisineToFood`) of cuisines to another map, which maps ratings to a sorted list of foods with that rating.

By organizing our data this way, we can maintain efficient updates and quick retrieval of top-rated foods for a given cuisine.

### Steps
1. Use a hash map (`foodToDetails`) to manage food, cuisine, and ratings.
2. Use a nested hash map (`cuisineToFood`) to quickly get the highest-rated food in any cuisine.
3. For updating ratings, manage the order of food within the cuisine hash maps.

### Code
```typescript
class FoodRatings {
  private foodToDetails: Map<string, { cuisine: string; rating: number }>;
  private cuisineToFood: Map<string, Map<number, Set<string>>>;

  constructor(foods: string[], cuisines: string[], ratings: number[]) {
    this.foodToDetails = new Map();
    this.cuisineToFood = new Map();

    for (let i = 0; i < foods.length; i++) {
      const food = foods[i];
      const cuisine = cuisines[i];
      const rating = ratings[i];

      // Initialize food details
      this.foodToDetails.set(food, { cuisine, rating });

      // Initialize cuisine group
      if (!this.cuisineToFood.has(cuisine)) {
        this.cuisineToFood.set(cuisine, new Map());
      }

      // Insert into cuisine food map
      if (!this.cuisineToFood.get(cuisine)!.has(rating)) {
        this.cuisineToFood.get(cuisine)!.set(rating, new Set());
      }
      this.cuisineToFood.get(cuisine)!.get(rating)!.add(food);
    }
  }

  changeRating(food: string, newRating: number): void {
    const { cuisine, rating: oldRating } = this.foodToDetails.get(food)!;
    
    // Update the rating in foodToDetails map
    this.foodToDetails.set(food, { cuisine, rating: newRating });

    // Remove from old rating
    this.cuisineToFood.get(cuisine)!.get(oldRating)!.delete(food);
    if (this.cuisineToFood.get(cuisine)!.get(oldRating)!.size === 0) {
      this.cuisineToFood.get(cuisine)!.delete(oldRating);
    }

    // Add to the new rating
    if (!this.cuisineToFood.get(cuisine)!.has(newRating)) {
      this.cuisineToFood.get(cuisine)!.set(newRating, new Set());
    }
    this.cuisineToFood.get(cuisine)!.get(newRating)!.add(food);
  }

  highestRated(cuisine: string): string {
    const ratings = this.cuisineToFood.get(cuisine)!;
    const highestRating = Math.max(...ratings.keys());

    return [...ratings.get(highestRating)!].reduce((a, b) => a < b ? a : b);
  }
}
```

### Complexity
- **Time Complexity**:
  - `changeRating()`: O(1) amortized.
  - `highestRated()`: O(m), where `m` is the number of unique ratings for that cuisine (typically much smaller than `n`).
- **Space Complexity**: O(n) for storing ratings and foods.

