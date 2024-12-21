# [Leetcode 729: My Calendar I](https://leetcode.com/problems/my-calendar-i/)

## Approaches:
- [Brute Force Solution](#brute-force-solution)
- [Optimized Solution with Binary Search Tree](#optimized-solution-with-binary-search-tree)

### Brute Force Solution

#### Intuition
The My Calendar I problem requires implementing a class that can book events without overlapping. The most direct approach is to maintain an array of intervals, and for each new booking request, check if it conflicts with any of the existing bookings.

#### Approach
- Use a list to keep a record of all valid booking intervals.
- For each new request, iterate through the list to check if it overlaps with any existing interval.
- If no overlap is found, add the new interval to the list and return `true`. Otherwise, return `false`.

#### Code
```typescript
class MyCalendar {
  private calendar: [number, number][];

  constructor() {
    this.calendar = [];
  }

  book(start: number, end: number): boolean {
    for (let [s, e] of this.calendar) {
      // Check for overlap: if the start is less than the current event's end
      // and the end is greater than the current event's start
      if (start < e && end > s) {
        return false;
      }
    }
    // No overlaps, booking is valid
    this.calendar.push([start, end]);
    return true;
  }
}

// Usage:
// const myCalendar = new MyCalendar();
// console.log(myCalendar.book(10, 20)); // returns true
// console.log(myCalendar.book(15, 25)); // returns false
```

#### Time Complexity
- The time complexity for booking each event is O(N), where N is the number of previously booked events. This is because we check every existing interval for overlap.

#### Space Complexity
- The space complexity is O(N) for storing up to N booked events.

### Optimized Solution with Binary Search Tree

#### Intuition
To improve efficiency, especially when the number of bookings increases, we can use a balanced tree data structure. This is because searching, inserting, and maintaining order are relatively slow in an unordered list compared to a sorted data structure when dealing with large datasets.

#### Approach
- Use a binary search tree to store the intervals.
- For each booking, take advantage of the property of BST to efficiently search for any overlapping intervals by comparing the start and end bounds while traversing the tree.

#### Code
```typescript
class TreeNode {
  start: number;
  end: number;
  left: TreeNode | null;
  right: TreeNode | null;

  constructor(start: number, end: number) {
    this.start = start;
    this.end = end;
    this.left = null;
    this.right = null;
  }
}

class MyCalendar {
  private root: TreeNode | null = null;

  book(start: number, end: number): boolean {
    const newEvent = new TreeNode(start, end);

    if (!this.root) {
      this.root = newEvent;
      return true;
    }

    return this.insert(this.root, newEvent);
  }

  private insert(root: TreeNode, newEvent: TreeNode): boolean {
    // Check for overlap
    if (newEvent.start < root.end && newEvent.end > root.start) {
      return false;
    }

    if (newEvent.start >= root.end) {
      // Recurse in the right subtree
      if (root.right) {
        return this.insert(root.right, newEvent);
      }
      root.right = newEvent;
      return true;
    } else {
      // Recurse in the left subtree
      if (root.left) {
        return this.insert(root.left, newEvent);
      }
      root.left = newEvent;
      return true;
    }
  }
}

// Usage:
// const myCalendar = new MyCalendar();
// console.log(myCalendar.book(10, 20)); // returns true
// console.log(myCalendar.book(15, 25)); // returns false
```

#### Time Complexity
- The time complexity is O(log N) on average, where N is the number of bookings because we're inserting into a balanced binary tree. However, in the worst case scenario (unbalanced tree), it could degrade to O(N).

#### Space Complexity
- The space complexity is O(N) for storing the nodes in the tree.

