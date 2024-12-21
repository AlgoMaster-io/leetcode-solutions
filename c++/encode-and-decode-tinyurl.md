# [Leetcode 535: Encode and Decode TinyURL](https://leetcode.com/problems/encode-and-decode-tinyurl/)

## Approaches

- [Approach 1: Hashing Using Random Generation](#approach-1-hashing-using-random-generation)
- [Approach 2: Sequential Mapping](#approach-2-sequential-mapping)
- [Approach 3: Base62 Encoding](#approach-3-base62-encoding)

---

## Approach 1: Hashing Using Random Generation

### Intuition

The basic idea of this approach is to map a long URL to a short URL using a random string generation technique. The unique identifier for each URL is created by generating a random combination of characters. This unique identifier is then stored in a map with the corresponding long URL. The major downside is the potential for hash collisions, which can be mitigated by regenerating the hash in case of a collision.

### Implementation

```cpp
#include <unordered_map>
#include <random>
#include <string>

class Solution {
public:
    std::unordered_map<std::string, std::string> urlMap;
    const std::string baseURL = "http://tinyurl.com/";
    const std::string charset = "0123456789abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ";
    const int shortURLLength = 6;

    std::string encode(std::string longUrl) {
        while (true) {
            std::string key = "";
            // Generate a random shortURL of length 6
            for (int i = 0; i < shortURLLength; ++i) {
                key += charset[rand() % charset.size()];
            }
            std::string shortUrl = baseURL + key;
            if (urlMap.find(key) == urlMap.end()) {
                // No collision, map this shortUrl to the longUrl
                urlMap[key] = longUrl;
                return shortUrl;
            }
        }
    }

    std::string decode(std::string shortUrl) {
        // Extract the key of length 6 and find the corresponding longUrl
        std::string key = shortUrl.substr(baseURL.size(), shortURLLength);
        return urlMap[key];
    }
};
```

### Time Complexity

- **Encoding**: O(1) on average for generating a unique key.
- **Decoding**: O(1) for retrieval from the map.

### Space Complexity

- O(n) where n is the number of URLs encoded.

---

## Approach 2: Sequential Mapping

### Intuition

This approach involves storing each URL with an incremented ID, starting from 1. The short URL is then created by adding this ID to the base URL. This method guarantees a unique short URL but requires careful handling of the ID's potential size, especially for very large datasets.

### Implementation

```cpp
#include <unordered_map>
#include <string>

class Solution {
public:
    std::unordered_map<int, std::string> urlMap;
    const std::string baseURL = "http://tinyurl.com/";
    int id = 1;

    std::string encode(std::string longUrl) {
        // Map the current ID to the provided long URL
        urlMap[id] = longUrl;
        return baseURL + std::to_string(id++);
    }

    std::string decode(std::string shortUrl) {
        // Extract the ID from the short URL
        int id = std::stoi(shortUrl.substr(baseURL.size()));
        return urlMap[id];
    }
};
```

### Time Complexity

- **Encoding**: O(1) because inserting a URL with its mapping takes constant time.
- **Decoding**: O(1) for direct map access.

### Space Complexity

- O(n) where n is the number of URLs encoded.

---

## Approach 3: Base62 Encoding

### Intuition

This method utilizes a numeric identifier for each URL and encodes this identifier in Base62 format. Base62 encoding uses digits, lowercase and uppercase letters, reducing the length of the encoded URL significantly compared to plain numeric strings.

### Implementation

```cpp
#include <unordered_map>
#include <string>

class Solution {
public:
    std::unordered_map<int, std::string> urlMap;
    std::unordered_map<std::string, int> reverseUrlMap;
    const std::string baseURL = "http://tinyurl.com/";
    const std::string charset = "0123456789abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ";
    int id = 1;

    std::string encode(std::string longUrl) {
        if (reverseUrlMap.find(longUrl) != reverseUrlMap.end()) {
            return baseURL + idToShortUrl(reverseUrlMap[longUrl]);
        }
        // Map id to the URL
        urlMap[id] = longUrl;
        reverseUrlMap[longUrl] = id;
        // Convert the ID to a Base62 string
        return baseURL + idToShortUrl(id++);
    }

    std::string decode(std::string shortUrl) {
        int id = shortUrlToId(shortUrl.substr(baseURL.size()));
        return urlMap[id];
    }

private:
    std::string idToShortUrl(int id) {
        std::string shortUrl;
        while (id > 0) {
            shortUrl = charset[id % 62] + shortUrl;
            id /= 62;
        }
        while (shortUrl.length() < 6) {
            shortUrl = '0' + shortUrl; // Pad the shortUrl to have a uniform length
        }
        return shortUrl;
    }

    int shortUrlToId(std::string shortUrl) {
        int id = 0;
        for (char c : shortUrl) {
            id = id * 62 + std::find(charset.begin(), charset.end(), c) - charset.begin();
        }
        return id;
    }
};
```

### Time Complexity

- **Encoding**: O(1) as we handle numbers and simple string manipulations.
- **Decoding**: O(1) for number decoding from Base62.
- **Note**: These complexities assume basic operations on strings and maps to be O(1).

### Space Complexity

- O(n) where n is the number of URLs, due to storage in maps.

