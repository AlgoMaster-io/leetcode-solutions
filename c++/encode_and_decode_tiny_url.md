# [Encode and Decode TinyURL](https://leetcode.com/problems/encode-and-decode-tinyurl/)

## Approach 1: HashMap with Auto-Increment ID

### Solution
```cpp
// Time Complexity: O(1) for both encode and decode
// Space Complexity: O(n) where n is the number of URLs encoded
#include <unordered_map>
#include <string>

class Codec {
public:
    Codec() : id(0) {}
    
    // Encodes a URL to a shortened URL.
    std::string encode(std::string longUrl) {
        id++;
        map[id] = longUrl;
        return "http://tinyurl.com/" + std::to_string(id); // Return the shortened URL
    }

    // Decodes a shortened URL to its original URL.
    std::string decode(std::string shortUrl) {
        int key = std::stoi(shortUrl.substr(19)); // Remove the "http://tinyurl.com/"
        return map[key]; // Retrieve the original URL
    }

private:
    std::unordered_map<int, std::string> map;
    int id;
};
```

## Approach 2: HashMap with Randomized Key

### Solution
```cpp
// Time Complexity: O(1) for both encode and decode
// Space Complexity: O(n) where n is the number of URLs encoded
#include <unordered_map>
#include <string>
#include <cstdlib>

class Codec {
public:
    // Encodes a URL to a shortened URL.
    std::string encode(std::string longUrl) {
        if (longToShort.count(longUrl)) {
            return "http://tinyurl.com/" + longToShort[longUrl];
        }
        std::string key;
        do {
            key = getRandomKey();
        } while (shortToLong.count(key)); // Ensure uniqueness of the key
        shortToLong[key] = longUrl;
        longToShort[longUrl] = key;
        return "http://tinyurl.com/" + key;
    }

    // Decodes a shortened URL to its original URL.
    std::string decode(std::string shortUrl) {
        std::string key = shortUrl.substr(19); // Remove the "http://tinyurl.com/"
        return shortToLong[key];
    }

private:
    std::unordered_map<std::string, std::string> shortToLong;
    std::unordered_map<std::string, std::string> longToShort;
    const std::string characters = "abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789";

    // Generate a random 6-character string
    std::string getRandomKey() {
        std::string key;
        for (int i = 0; i < 6; ++i) {
            key += characters[std::rand() % characters.size()];
        }
        return key;
    }
};
```

## Approach 3: Hashing with Fixed-Length Encoding

### Solution
```cpp
// Time Complexity: O(1) for both encode and decode
// Space Complexity: O(n) where n is the number of URLs encoded
#include <unordered_map>
#include <string>

class Codec {
public:
    Codec() : id(0), BASE(characters.size()) {}
    
    // Encodes a URL to a shortened URL.
    std::string encode(std::string longUrl) {
        id++;
        std::string shortKey = toBase62(id);
        map[shortKey] = longUrl;
        return "http://tinyurl.com/" + shortKey;
    }

    // Decodes a shortened URL to its original URL.
    std::string decode(std::string shortUrl) {
        std::string key = shortUrl.substr(19); // Remove the "http://tinyurl.com/"
        return map[key];
    }

private:
    std::unordered_map<std::string, std::string> map;
    const std::string characters = "abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789";
    const int BASE;
    int id;

    // Generate a hash-like key for the URL
    std::string toBase62(int num) {
        std::string encoded;
        while (num > 0) {
            encoded = characters[num % BASE] + encoded;
            num /= BASE;
        }
        return encoded;
    }

    int toBase10(const std::string& base62) {
        int num = 0;
        for (char c : base62) {
            num = num * BASE + characters.find(c);
        }
        return num;
    }
};
```

