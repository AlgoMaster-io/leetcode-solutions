# [Leetcode 355: Design Twitter](https://leetcode.com/problems/design-twitter/)

## Approaches

- [Approach 1: Basic Design with Lists and Linear Search](#approach-1-basic-design-with-lists-and-linear-search)
- [Approach 2: Optimized Design with Priority Queue](#approach-2-optimized-design-with-priority-queue)

### Approach 1: Basic Design with Lists and Linear Search

#### Intuition

The problem requires implementing a Twitter-like system where users can post tweets, follow other users, and retrieve a news feed consisting of tweets from those they follow plus their own. A basic approach to implement this uses lists to store tweets and follows relationships, and a linear search to compile the news feed. Each tweet is paired with a timestamp to maintain the order of posting.

#### Implementation

```go
type Twitter struct {
    tweets      map[int][]Tweet  // map of userId to list of tweets with timestamps
    followings  map[int]map[int]bool // map of userId to set of followeeIds
    time        int              // global timestamp counter
}

type Tweet struct {
    id   int // Tweet id
    time int // Timestamp of the tweet
}

func Constructor() Twitter {
    return Twitter{
        tweets:     make(map[int][]Tweet),
        followings: make(map[int]map[int]bool),
        time:       0,
    }
}

func (this *Twitter) PostTweet(userId int, tweetId int) {
    this.time++
    this.tweets[userId] = append(this.tweets[userId], Tweet{id: tweetId, time: this.time})
}

func (this *Twitter) GetNewsFeed(userId int) []int {
    newsFeed := []Tweet{}
    
    // Get own tweets
    newsFeed = append(newsFeed, this.tweets[userId]...)
    
    // Get tweets from followees
    if follows, exists := this.followings[userId]; exists {
        for followee := range follows {
            newsFeed = append(newsFeed, this.tweets[followee]...)
        }
    }
    
    // Sort tweets by time in descending order
    sort.Slice(newsFeed, func(i, j int) bool {
        return newsFeed[i].time > newsFeed[j].time
    })
    
    // Get top 10 tweets
    limit := 10
    if len(newsFeed) < 10 {
        limit = len(newsFeed)
    }
    
    feed := make([]int, limit)
    for i := 0; i < limit; i++ {
        feed[i] = newsFeed[i].id
    }
    
    return feed
}

func (this *Twitter) Follow(followerId int, followeeId int) {
    if this.followings[followerId] == nil {
        this.followings[followerId] = make(map[int]bool)
    }
    this.followings[followerId][followeeId] = true
}

func (this *Twitter) Unfollow(followerId int, followeeId int) {
    if this.followings[followerId] != nil {
        delete(this.followings[followerId], followeeId)
    }
}
```

#### Time Complexity

- `PostTweet`: O(1) to append a tweet to the list.
- `GetNewsFeed`: O(n + k * log(k)), where `n` is the number of tweets collected for the feed and `k` is the number of tweets (max 10) that are sorted.
- `Follow` and `Unfollow`: O(1) for adding or removing an entry from a map.

#### Space Complexity

O(u + n), where `u` is the number of users and `n` is the total number of tweets.

### Approach 2: Optimized Design with Priority Queue

#### Intuition

To optimize the process of retrieving the top 10 most recent tweets for the news feed, a priority queue (min-heap) can be used. This allows us to efficiently keep track of the top tweets in terms of timestamp, and we can use this to retrieve the news feed.

#### Implementation

```go
import (
    "container/heap"
)

type Twitter struct {
    tweets      map[int][]Tweet
    followings  map[int]map[int]bool
    time        int
}

type Tweet struct {
    id   int
    time int
}

type PriorityQueue []Tweet

func (pq PriorityQueue) Len() int {
    return len(pq)
}

func (pq PriorityQueue) Less(i, j int) bool {
    return pq[i].time > pq[j].time // Max-Heap by time
}

func (pq PriorityQueue) Swap(i, j int) {
    pq[i], pq[j] = pq[j], pq[i]
}

func (pq *PriorityQueue) Push(x interface{}) {
    *pq = append(*pq, x.(Tweet))
}

func (pq *PriorityQueue) Pop() interface{} {
    old := *pq
    n := len(old)
    item := old[n-1]
    *pq = old[0 : n-1]
    return item
}

func Constructor() Twitter {
    return Twitter{
        tweets:     make(map[int][]Tweet),
        followings: make(map[int]map[int]bool),
        time:       0,
    }
}

func (this *Twitter) PostTweet(userId int, tweetId int) {
    this.time++
    this.tweets[userId] = append(this.tweets[userId], Tweet{id: tweetId, time: this.time})
}

func (this *Twitter) GetNewsFeed(userId int) []int {
    pq := &PriorityQueue{}
    heap.Init(pq)
    
    // Collect tweets from the user and their followees
    users := append([]int{userId}, this.followees(userId)...)
    
    for _, usr := range users {
        for _, tweet := range this.tweets[usr] {
            heap.Push(pq, tweet)
            if pq.Len() > 10 {
                heap.Pop(pq)
            }
        }
    }
    
    // Build and return the feed
    feed := make([]int, pq.Len())
    for i := pq.Len() - 1; i >= 0; i-- {
        feed[i] = heap.Pop(pq).(Tweet).id
    }
    
    return feed
}

func (this *Twitter) followees(userId int) []int {
    // Convert followings map to a slice of followee ids
    followees := []int{}
    if follows, exists := this.followings[userId]; exists {
        for followee := range follows {
            followees = append(followees, followee)
        }
    }
    return followees
}

func (this *Twitter) Follow(followerId int, followeeId int) {
    if this.followings[followerId] == nil {
        this.followings[followerId] = make(map[int]bool)
    }
    this.followings[followerId][followeeId] = true
}

func (this *Twitter) Unfollow(followerId int, followeeId int) {
    if this.followings[followerId] != nil {
        delete(this.followings[followerId], followeeId)
    }
}
```

#### Time Complexity

- `PostTweet`: O(1)
- `GetNewsFeed`: O(u * m * log(10)) where `u` is the number of users in the follower list (including the user) and `m` is the average number of tweets per user. Logarithmic factor is due to the heap operations.
- `Follow` and `Unfollow`: O(1)

#### Space Complexity

O(u + n), where `u` is the number of users, and `n` is the total number of tweets.

