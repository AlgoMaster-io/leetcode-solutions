# [Leetcode 355: Design Twitter](https://leetcode.com/problems/design-twitter/)

## Solutions
- [Naive Approach](#naive-approach)
- [Optimized Approach Using HashMaps and Min-Heap](#optimized-approach-using-hashmaps-and-min-heap)

### Naive Approach

**Intuition:**

The basic idea is to maintain a list of tweets for every user, so that when someone posts a tweet, it gets appended to their list. For fetching the news feed, we combine the lists of all users the current user follows, and then sort them to get the most recent tweets. This approach is intuitive but inefficient because the sorting step can be slow if the number of tweets is large.

**Steps:**

1. Use a map (`userTweets`) to store lists of tweets for each user.
2. Another map (`userFollows`) tracks which users are followed by whom.
3. When `postTweet` is called, append the new tweet to the user's list.
4. When `getNewsFeed` is called, gather all tweets from the users being followed, sort by time, and return the top 10 most recent.
5. Implement `follow` and `unfollow` to update the `userFollows` map accordingly.

**Time Complexity:**

- **postTweet:** O(1)
- **getNewsFeed:** O(N \* log(N)) where N is the total number of tweets from followed users (due to sorting).
- **follow/unfollow:** O(1)

**Space Complexity:** O(U + T) where U is the number of users and T is the number of tweets.

```typescript
type Tweet = {
  id: number;
  time: number;
};

class Twitter {
  private userTweets: Map<number, Tweet[]>;
  private userFollows: Map<number, Set<number>>;
  private timeStamp: number;

  constructor() {
    this.userTweets = new Map();
    this.userFollows = new Map();
    this.timeStamp = 0;
  }

  postTweet(userId: number, tweetId: number): void {
    if (!this.userTweets.has(userId)) {
      this.userTweets.set(userId, []);
    }
    this.userTweets.get(userId)!.push({ id: tweetId, time: this.timeStamp++ });
  }

  getNewsFeed(userId: number): number[] {
    const tweets: Tweet[] = [];
    if (this.userTweets.has(userId)) {
      tweets.push(...this.userTweets.get(userId)!);
    }

    const followedUsers = this.userFollows.get(userId) || new Set();
    for (const followeeId of followedUsers) {
      if (this.userTweets.has(followeeId)) {
        tweets.push(...this.userTweets.get(followeeId)!);
      }
    }

    tweets.sort((a, b) => b.time - a.time);
    return tweets.slice(0, 10).map(tweet => tweet.id);
  }

  follow(followerId: number, followeeId: number): void {
    if (!this.userFollows.has(followerId)) {
      this.userFollows.set(followerId, new Set());
    }
    this.userFollows.get(followerId)!.add(followeeId);
  }

  unfollow(followerId: number, followeeId: number): void {
    if (this.userFollows.has(followerId)) {
      this.userFollows.get(followerId)!.delete(followeeId);
    }
  }
}
```

### Optimized Approach Using HashMaps and Min-Heap

**Intuition:**

To improve efficiency, especially for fetching news feeds, we use a different data structure—a min-heap—allowing us to maintain a fixed-size collection of the top 10 recent tweets. This approach avoids sorting a potentially large list of tweets every time `getNewsFeed` is called.

**Steps:**

1. **Data Structures:**
   - Continue using a map for `userTweets` and `userFollows`.
   - Introduce a priority queue (min-heap) to track the top 10 recent tweets.

2. **Operations:**
   - **postTweet:** Same as before, keeping track of the timestamp.
   - **getNewsFeed:** Use a min-heap to efficiently keep track of the 10 most recent tweets.
   - **follow/unfollow:** Similarly, manage the followers using the map.

**Time Complexity:**

- **postTweet:** O(1)
- **getNewsFeed:** O(U \* log(10)) = O(U) where U is the number of followed users (because heap operations are logarithmic in constant size).
- **follow/unfollow:** O(1)

**Space Complexity:** O(U + T)

This strategy significantly reduces the time complexity of `getNewsFeed` in comparison to sorting.

```typescript
class MinHeap<T> {
  private heap: T[];
  private comparator: (a: T, b: T) => number;

  constructor(comparator: (a: T, b: T) => number) {
    this.heap = [];
    this.comparator = comparator;
  }

  offer(value: T): void {
    this.heap.push(value);
    this.heapifyUp(this.size() - 1);
  }

  poll(): T | undefined {
    if (this.size() === 0) return undefined;
    const root = this.heap[0];
    const last = this.heap.pop();
    if (this.size() > 0 && last !== undefined) {
      this.heap[0] = last;
      this.heapifyDown(0);
    }
    return root;
  }

  peek(): T | undefined {
    return this.heap[0];
  }

  size(): number {
    return this.heap.length;
  }

  private heapifyUp(index: number): void {
    while (index > 0) {
      const parent = Math.floor((index - 1) / 2);
      if (this.comparator(this.heap[parent], this.heap[index]) <= 0) break;
      [this.heap[parent], this.heap[index]] = [this.heap[index], this.heap[parent]];
      index = parent;
    }
  }

  private heapifyDown(index: number): void {
    const size = this.size();
    while (true) {
      let left = 2 * index + 1;
      let right = 2 * index + 2;
      let smallest = index;
      if (left < size && this.comparator(this.heap[left], this.heap[smallest]) < 0) smallest = left;
      if (right < size && this.comparator(this.heap[right], this.heap[smallest]) < 0) smallest = right;
      if (index === smallest) break;
      [this.heap[index], this.heap[smallest]] = [this.heap[smallest], this.heap[index]];
      index = smallest;
    }
  }
}

class Twitter {
  private userTweets: Map<number, Tweet[]>;
  private userFollows: Map<number, Set<number>>;
  private timeStamp: number;

  constructor() {
    this.userTweets = new Map();
    this.userFollows = new Map();
    this.timeStamp = 0;
  }

  postTweet(userId: number, tweetId: number): void {
    if (!this.userTweets.has(userId)) {
      this.userTweets.set(userId, []);
    }
    this.userTweets.get(userId)!.push({ id: tweetId, time: this.timeStamp++ });
  }

  getNewsFeed(userId: number): number[] {
    const minHeap = new MinHeap<Tweet>((a, b) => a.time - b.time); // Min-heap based on timestamp
    const includedUsers = this.userFollows.get(userId) || new Set();
    includedUsers.add(userId); // Include own tweets

    for (const uid of includedUsers) {
      const tweets = this.userTweets.get(uid) || [];
      for (const tweet of tweets) {
        minHeap.offer(tweet);
        if (minHeap.size() > 10) {
          minHeap.poll(); // Maintain only the top 10 items
        }
      }
    }

    const result: number[] = [];
    while (minHeap.size() > 0) {
      result.push(minHeap.poll()!.id);
    }
    return result.reverse(); // Since min-heap, reverse to get sorted in decreasing order
  }

  follow(followerId: number, followeeId: number): void {
    if (!this.userFollows.has(followerId)) {
      this.userFollows.set(followerId, new Set());
    }
    this.userFollows.get(followerId)!.add(followeeId);
  }

  unfollow(followerId: number, followeeId: number): void {
    if (this.userFollows.has(followerId)) {
      this.userFollows.get(followerId)!.delete(followeeId);
    }
  }
}
```

This optimized solution now allows fetching the news feed efficiently using a min-heap, ensuring the operation scales better with the number of tweets in the system.

