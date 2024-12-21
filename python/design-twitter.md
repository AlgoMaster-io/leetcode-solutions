# [355. Design Twitter](https://leetcode.com/problems/design-twitter/)

## Approaches:
- [Basic Approach: Maintain User and Tweet Data Structures](#basic-approach)
- [Optimized Approach: Use Priority Queues](#optimized-approach)

---

### Basic Approach: Maintain User and Tweet Data Structures

#### Intuition:
The problem requires developing a simplified version of Twitter where users can post tweets, follow other users, and retrieve the latest tweets from their feeds. We can achieve this with a straightforward data structure approach:

1. Maintain two main data structures:
   - A dictionary to store which users follow which others.
   - A dictionary to store each user's tweets.

2. Implement each functionality as follows:
   - `postTweet`: Add the tweet to the user's list of tweets.
   - `getNewsFeed`: Gather tweets from the user and those they follow, sort them in reverse chronological order and return the top 10.
   - `follow`/`unfollow`: Modify the follower list accordingly.

#### Time Complexity:
- `postTweet`: \(O(1)\)
- `getNewsFeed`: \(O(N \log N)\) where \(N\) is the number of tweets in the aggregate feed (sorting step).
- `follow`/`unfollow`: \(O(1)\)

#### Space Complexity:
- \(O(U + T)\) where \(U\) is the number of users and \(T\) is the number of tweets.

```python
class Twitter:
    def __init__(self):
        # Initialize the follower map
        self.followers = {}
        # Initialize tweets map
        self.tweets = {}
        self.timestamp = 0

    def postTweet(self, userId: int, tweetId: int) -> None:
        # Append the tweet with the increasing timestamp
        if userId not in self.tweets:
            self.tweets[userId] = []
        self.tweets[userId].append((self.timestamp, tweetId))
        self.timestamp += 1

    def getNewsFeed(self, userId: int) -> [int]:
        # Retrieve the tweets of the user and users they follow
        news_feed = []
        
        # Add user's tweets
        tweets_to_check = self.tweets.get(userId, [])
        
        # Add followed users' tweets
        if userId in self.followers:
            for followee in self.followers[userId]:
                tweets_to_check.extend(self.tweets.get(followee, []))
        
        # Sort by timestamp and get the most recent 10 
        news_feed = sorted(tweets_to_check, key=lambda x: x[0], reverse=True)[:10]
        
        return [tweet[1] for tweet in news_feed]

    def follow(self, followerId: int, followeeId: int) -> None:
        if followerId not in self.followers:
            self.followers[followerId] = set()
        self.followers[followerId].add(followeeId)

    def unfollow(self, followerId: int, followeeId: int) -> None:
        if followerId in self.followers:
            self.followers[followerId].discard(followeeId)
```

---

### Optimized Approach: Use Priority Queues

#### Intuition:
Random access and retrieval efficiency can be improved by using priority queues to manage and sort tweets for retrieval. Since the primary operation of news feeds involves handling time-based tweet ordering, leveraging a heap solves this efficiently:

1. Each `postTweet` operation increases a global timestamp and adds a detailed timestamped tweet for ordering.
2. The `getNewsFeed` utilizes a max-heap to manage sorting on the fly instead of sorting all possible tweets every time.
3. Priority queues (heaps) allow efficient retrieval of the top 10 most recent tweets based on timestamp.

#### Time Complexity:
- `postTweet`: \(O(1)\)
- `getNewsFeed`: \(O(M \log 10)\) where \(M\) is total tweets from user and user's followees, optimized to \(O(M)\) since the heap is capped at 10.
- `follow`/`unfollow`: \(O(1)\)

#### Space Complexity:
- \(O(U + T)\)

```python
import heapq

class Twitter:
    def __init__(self):
        self.user_tweets = {}
        self.followers = {}
        self.timestamp = 0

    def postTweet(self, userId: int, tweetId: int) -> None:
        if userId not in self.user_tweets:
            self.user_tweets[userId] = []
        self.user_tweets[userId].append((self.timestamp, tweetId))
        self.timestamp += 1

    def getNewsFeed(self, userId: int) -> [int]:
        min_heap = []
        tweets_to_check = list(self.user_tweets.get(userId, []))
        
        if userId in self.followers:
            for followee in self.followers[userId]:
                tweets_to_check.extend(self.user_tweets.get(followee, []))
        
        for tweet in tweets_to_check:
            heapq.heappush(min_heap, (-tweet[0], tweet[1]))
            if len(min_heap) > 10:
                heapq.heappop(min_heap)

        return [heapq.heappop(min_heap)[1] for _ in range(len(min_heap))][::-1]

    def follow(self, followerId: int, followeeId: int) -> None:
        if followerId == followeeId:
            return
        if followerId not in self.followers:
            self.followers[followerId] = set()
        self.followers[followerId].add(followeeId)

    def unfollow(self, followerId: int, followeeId: int) -> None:
        if followerId in self.followers:
            self.followers[followerId].discard(followeeId)
```

---

This provides an efficient solution to modeling a simplified Twitter platform using basic and optimized data structuring techniques.

