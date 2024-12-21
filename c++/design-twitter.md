# [Leetcode 355: Design Twitter](https://leetcode.com/problems/design-twitter/)

### Approaches
1. [Basic Implementation with Vector and Unordered Map](#approach-1-basic-implementation-with-vector-and-unordered-map)
2. [Priority Queue for Optimized Feed Retrieval](#approach-2-priority-queue-for-optimized-feed-retrieval)

---

## Approach 1: Basic Implementation with Vector and Unordered Map

### Intuition
The problem requires creating a social media platform that can post Tweets, follow/unfollow users, and retrieve the latest tweets. The direct approach involves using simple data structures to manage the functionalities.

We can use:
- A `vector` to store tweets with their timestamps.
- Two `unordered_map`s:
  - One to map each user to their followers.
  - Another to store the tweets posted by each user.

This straightforward approach will allow us to simulate most functionalities, albeit not the most efficiently for larger datasets.

### Implementation
```cpp
#include <vector>
#include <unordered_map>
#include <unordered_set>
#include <algorithm>

class Twitter {
    using Tweet = std::pair<int, int>; // (timestamp, tweetId)
    std::unordered_map<int, std::unordered_set<int>> followers; // userId -> set of followers
    std::unordered_map<int, std::vector<Tweet>> tweets; // userId -> vector of (timestamp, tweetId)
    int timestamp;

public:
    Twitter() : timestamp(0) {}

    void postTweet(int userId, int tweetId) {
        tweets[userId].emplace_back(timestamp++, tweetId);
    }

    std::vector<int> getNewsFeed(int userId) {
        // Collect tweets from the user and their followers
        std::vector<Tweet> feed;
        
        // Include user's own tweets
        for (const Tweet& tweet : tweets[userId]) {
            feed.push_back(tweet);
        }
        
        // Include tweets from followers
        for (int followee : followers[userId]) {
            for (const Tweet& tweet : tweets[followee]) {
                feed.push_back(tweet);
            }
        }
        
        // Sort by timestamp descending and take at most 10
        std::sort(feed.begin(), feed.end(), [](const Tweet& a, const Tweet& b) {
            return a.first > b.first;
        });
        
        std::vector<int> result;
        for (int i = 0; i < feed.size() && i < 10; ++i) {
            result.push_back(feed[i].second);
        }
        return result;
    }

    void follow(int followerId, int followeeId) {
        if (followerId != followeeId) {
            followers[followerId].insert(followeeId);
        }
    }

    void unfollow(int followerId, int followeeId) {
        followers[followerId].erase(followeeId);
    }
};
```

### Complexity
- **Time Complexity**: 
  - Post Tweet: O(1)
  - Follow/Unfollow: O(1)
  - Get News Feed: O(N log N), where N is the total number of tweets from the user and their followers.

- **Space Complexity**: O(N), where N is the total number of tweets stored.

## Approach 2: Priority Queue for Optimized Feed Retrieval

### Intuition
We can further optimize the news feed retrieval using a priority queue (or a max heap), which allows fetching top elements without the need to sort the entire list. By retrieving the 10 latest tweets efficiently, our approach improves performance, especially useful when the number of tweets grows very large.

Instead of sorting all tweets, a priority queue can maintain the top 10, removing the smallest whenever a new tweet timestamp is greater than the smallest in the queue.

### Implementation
```cpp
#include <vector>
#include <unordered_map>
#include <unordered_set>
#include <queue>

class Twitter {
    using Tweet = std::pair<int, int>; // (timestamp, tweetId)
    std::unordered_map<int, std::unordered_set<int>> followers; // userId -> set of followers
    std::unordered_map<int, std::vector<Tweet>> tweets; // userId -> vector of (timestamp, tweetId)
    int timestamp;

public:
    Twitter() : timestamp(0) {}

    void postTweet(int userId, int tweetId) {
        tweets[userId].emplace_back(timestamp++, tweetId);
    }

    std::vector<int> getNewsFeed(int userId) {
        auto cmp = [](const Tweet& a, const Tweet& b) {
            return a.first < b.first; // Min-heap based on timestamp
        };
        std::priority_queue<Tweet, std::vector<Tweet>, decltype(cmp)> pq(cmp);

        // Add user's own tweets to the min-heap
        for (const Tweet& tweet : tweets[userId]) {
            pq.push(tweet);
            if (pq.size() > 10) pq.pop();
        }

        // Add tweets from user's followees to the min-heap
        for (int followeeId : followers[userId]) {
            for (const Tweet& tweet : tweets[followeeId]) {
                pq.push(tweet);
                if (pq.size() > 10) pq.pop();
            }
        }

        // Collect the tweets from the priority queue
        std::vector<int> result(pq.size());
        for (int i = pq.size() - 1; i >= 0; --i) {
            result[i] = pq.top().second;
            pq.pop();
        }
        return result;
    }

    void follow(int followerId, int followeeId) {
        if (followerId != followeeId) {
            followers[followerId].insert(followeeId);
        }
    }

    void unfollow(int followerId, int followeeId) {
        followers[followerId].erase(followeeId);
    }
};
```

### Complexity
- **Time Complexity**: 
  - Post Tweet: O(1)
  - Follow/Unfollow: O(1)
  - Get News Feed: O(N log 10) ≈ O(N), where N is the total number of tweets from the user and their followers due to maintaining a heap of size 10.

- **Space Complexity**: O(N), where N is the total number of tweets stored.

This second approach provides a more efficient method for retrieving the latest tweets using a priority queue, particularly beneficial in applications with a vast amount of tweets and users.

