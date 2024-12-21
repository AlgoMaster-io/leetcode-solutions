# [LeetCode 355: Design Twitter](https://leetcode.com/problems/design-twitter/)

## Approaches:
- [Naive Approach](#naive-approach)
- [Optimized Approach using Min-Heap](#optimized-approach-using-min-heap)

### Naive Approach
#### Intuition:
This naive approach involves maintaining three main data structures:
1. **Map of Tweets**: A HashMap to store all tweets for each user.
2. **Follow Graph**: A HashMap to store follow relationships.
3. **Global Tweet List**: A list of all tweets in the order they were posted.

Using these data structures, we can retrieve the latest tweets for any user by filtering the global tweet list according to the follow relationships.

#### Code:
```java
import java.util.*;

class Twitter {
    private Map<Integer, Set<Integer>> followMap;
    private Map<Integer, List<int[]>> tweetMap;
    private List<int[]> tweetList;
    private int timeStamp;
    
    public Twitter() {
        followMap = new HashMap<>();
        tweetMap = new HashMap<>();
        tweetList = new ArrayList<>();
        timeStamp = 0;
    }
    
    public void postTweet(int userId, int tweetId) {
        // Add the tweet to the user's tweet list
        tweetMap.putIfAbsent(userId, new ArrayList<>());
        tweetMap.get(userId).add(new int[]{tweetId, timeStamp++});
        // Add the tweet to the global tweet list
        tweetList.add(new int[]{tweetId, userId});
    }
    
    public List<Integer> getNewsFeed(int userId) {
        List<Integer> result = new LinkedList<>();
        Set<Integer> followees = followMap.getOrDefault(userId, new HashSet<>());
        
        int count = 0;
        for(int i = tweetList.size() - 1; i >= 0 && count < 10; i--) {
            int[] tweet = tweetList.get(i);
            int tweetId = tweet[0];
            int user = tweet[1];
            if(user == userId || followees.contains(user)) {
                result.add(tweetId);
                count++;
            }
        }
        
        return result;
    }
    
    public void follow(int followerId, int followeeId) {
        followMap.putIfAbsent(followerId, new HashSet<>());
        followMap.get(followerId).add(followeeId);
    }
    
    public void unfollow(int followerId, int followeeId) {
        if(followerId == followeeId) return; // Prevent unfollowing oneself
        Set<Integer> followees = followMap.get(followerId);
        if(followees != null) {
            followees.remove(followeeId);
        }
    }
}
```

#### Time Complexity:
- Posting a tweet: O(1).
- Getting the news feed: O(n) where n is the number of tweets (limited to the recent ones, i.e., last 10).
- Follow and unfollow: O(1).

#### Space Complexity:
- O(u + t) where u is the number of users and t is the number of tweets.

### Optimized Approach using Min-Heap
#### Intuition:
To improve the retrieval of the latest tweets for a user, we utilize a Min-Heap to efficiently manage the fetching of the top 10 most recent tweets. We maintain similar data structures for storing tweets and follow relationships, but enhance the retrieval by using the current timestamp to prioritize recent tweets.

#### Code:
```java
import java.util.*;

class Twitter {
    private Map<Integer, Set<Integer>> followGraph;
    private Map<Integer, List<int[]>> userTweets;
    private int timeStamp;
    
    public Twitter() {
        followGraph = new HashMap<>();
        userTweets = new HashMap<>();
        timeStamp = 0;
    }
    
    public void postTweet(int userId, int tweetId) {
        userTweets.putIfAbsent(userId, new ArrayList<>());
        userTweets.get(userId).add(new int[]{tweetId, timeStamp++});
    }
    
    public List<Integer> getNewsFeed(int userId) {
        PriorityQueue<int[]> minHeap = new PriorityQueue<>((a, b) -> a[1] - b[1]);
        Set<Integer> followees = followGraph.getOrDefault(userId, new HashSet<>());
        followees.add(userId); // Include user's own tweets
        
        for(int followeeId : followees) {
            List<int[]> tweets = userTweets.getOrDefault(followeeId, new ArrayList<>());
            for(int[] tweet : tweets) {
                minHeap.add(tweet);
                if(minHeap.size() > 10) {
                    minHeap.poll();
                }
            }
        }
        
        List<Integer> result = new LinkedList<>();
        while (!minHeap.isEmpty()) {
            result.add(0, minHeap.poll()[0]);
        }
        return result;
    }
    
    public void follow(int followerId, int followeeId) {
        followGraph.putIfAbsent(followerId, new HashSet<>());
        followGraph.get(followerId).add(followeeId);
    }
    
    public void unfollow(int followerId, int followeeId) {
        if(followerId != followeeId && followGraph.containsKey(followerId)) {
            followGraph.get(followerId).remove(followeeId);
        }
    }
}
```

#### Time Complexity:
- Posting a tweet: O(1).
- Getting the news feed: O(n log 10) where n is the number of tweets by followers but it is limited to 10 recent tweets.
- Follow and unfollow: O(1).

#### Space Complexity:
- O(u + t) where u is the number of users and t is the number of tweets. The additional overhead is due to the priority queue holding at most 10 elements.

