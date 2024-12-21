# [Leetcode 355: Design Twitter](https://leetcode.com/problems/design-twitter/)

## Approaches
- [Approach 1: Naive Solution using Lists](#approach-1-naive-solution-using-lists)
- [Approach 2: Optimized Solution using Dictionary and Priority Queue](#approach-2-optimized-solution-using-dictionary-and-priority-queue)

### Approach 1: Naive Solution using Lists

#### Intuition:
In the naive solution, the key is to manage users, their posted tweets, and their followers. We can utilize a straightforward list-based approach for this. Every user can have their list of tweets, and we maintain another list capturing follower-followee relationships. When retrieving the most recent tweets, we can scan through the necessary lists. This approach, although direct, is not very efficient, especially with a larger number of users or tweets.

#### Steps:
1. **Data Structure Initialization**: Use lists to store tweets and follower-followee relationships.

2. **Post Tweets**: Append each tweet to the user's list. Associate each tweet with a timestamp for ordering.

3. **Follow/Unfollow**: Modify the follower-followee list accordingly.

4. **Get News Feed**: For the current user:
   - Gather all tweets from themselves and those they follow.
   - Sort and return the latest `10` tweets.

```csharp
public class Twitter {
    private List<(int userId, int tweetId, int timestamp)> tweets;
    private Dictionary<int, HashSet<int>> followers;
    private int time;

    public Twitter() {
        tweets = new List<(int userId, int tweetId, int timestamp)>();
        followers = new Dictionary<int, HashSet<int>>();
        time = 0;
    }

    public void PostTweet(int userId, int tweetId) {
        tweets.Add((userId, tweetId, time++));
    }

    public IList<int> GetNewsFeed(int userId) {
        var feed = new List<(int tweetId, int timestamp)>();
        
        // Add user's tweets
        foreach (var tweet in tweets) {
            if (tweet.userId == userId) {
                feed.Add((tweet.tweetId, tweet.timestamp));
            }
        }

        // Add followees' tweets
        if (followers.ContainsKey(userId)) {
            foreach (int followeeId in followers[userId]) {
                foreach (var tweet in tweets) {
                    if (tweet.userId == followeeId) {
                        feed.Add((tweet.tweetId, tweet.timestamp));
                    }
                }
            }
        }

        // Sort based on timestamp in descending order and take the latest 10
        feed.Sort((a, b) => b.timestamp.CompareTo(a.timestamp));
        return feed.Take(10).Select(t => t.tweetId).ToList();
    }

    public void Follow(int followerId, int followeeId) {
        if (!followers.ContainsKey(followerId)) {
            followers[followerId] = new HashSet<int>();
        }
        followers[followerId].Add(followeeId);
    }

    public void Unfollow(int followerId, int followeeId) {
        if (followers.ContainsKey(followerId)) {
            followers[followerId].Remove(followeeId);
        }
    }
}
```

**Time Complexity**: 
- `PostTweet`: O(1) - Adding a tweet.
- `GetNewsFeed`: O(N log N) - Sorting the tweets (N is the number of tweets).
- `Follow/Unfollow`: O(1) - Adding/removing users.

**Space Complexity**: 
- O(U + T) where U is the number of users and T is the number of tweets.

### Approach 2: Optimized Solution using Dictionary and Priority Queue

#### Intuition:
The naive solution does not efficiently handle large datasets. By leveraging a dictionary to store users' tweets and a priority queue to fetch the most recent tweets, we can enhance performance. Dictionaries offer quick access to each user's tweets, while a priority queue efficiently sorts tweets to provide the latest.

#### Steps:
1. **Data Structure Initialization**: Use a dictionary for each user's tweets. Use a dictionary for follower relationships. Utilize a priority queue to manage tweet sorting for newsfeeds.

2. **Post Tweets**: Update the dictionary with user's tweets tagged with a timestamp.

3. **Follow/Unfollow**: Modify follower-followee relationships efficiently using dictionaries.

4. **Get News Feed**: For the current user:
   - Compile tweets from themselves and those they follow using a priority queue to maintain order.
   - Extract up to the 10 latest in priority order.

```csharp
public class Twitter {
    private int time;
    private Dictionary<int, List<(int tweetId, int timestamp)>> userTweets;
    private Dictionary<int, HashSet<int>> followers;

    public Twitter() {
        time = 0;
        userTweets = new Dictionary<int, List<(int tweetId, int timestamp)>>();
        followers = new Dictionary<int, HashSet<int>>();
    }

    public void PostTweet(int userId, int tweetId) {
        if (!userTweets.ContainsKey(userId)) {
            userTweets[userId] = new List<(int tweetId, int timestamp)>();
        }
        userTweets[userId].Add((tweetId, time++));
    }

    public IList<int> GetNewsFeed(int userId) {
        SortedList<int, int> feed = new SortedList<int, int>(Comparer<int>.Create((a, b) => b.CompareTo(a)));
        
        // Add user's tweets
        if (userTweets.ContainsKey(userId)) {
            foreach (var tweet in userTweets[userId]) {
                feed.Add(tweet.timestamp, tweet.tweetId);
            }
        }

        // Add followees' tweets
        if (followers.ContainsKey(userId)) {
            foreach (int followeeId in followers[userId]) {
                if (userTweets.ContainsKey(followeeId)) {
                    foreach (var tweet in userTweets[followeeId]) {
                        feed.Add(tweet.timestamp, tweet.tweetId);
                    }
                }
            }
        }

        return feed.Values.Take(10).ToList();
    }

    public void Follow(int followerId, int followeeId) {
        if (!followers.ContainsKey(followerId)) {
            followers[followerId] = new HashSet<int>();
        }
        followers[followerId].Add(followeeId);
    }

    public void Unfollow(int followerId, int followeeId) {
        if (followers.ContainsKey(followerId)) {
            followers[followerId].Remove(followeeId);
        }
    }
}
```

**Time Complexity**: 
- `PostTweet`: O(1) - Adding a tweet.
- `GetNewsFeed`: O(M log K) - M is all retrieved tweets, K = 10 from the news feed. Priority queue allows efficient retrieval.
- `Follow/Unfollow`: O(1) - Using hash sets.

**Space Complexity**: 
- O(U + T) where U is the number of users and T is total tweets due to stored tweet lists.

The optimized solution significantly reduces the time taken to collect and sort tweets, allowing for a more responsive system, especially beneficial for more extensive user bases and tweet activity.

