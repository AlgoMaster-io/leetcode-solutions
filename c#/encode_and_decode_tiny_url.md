# [Encode and Decode TinyURL](https://leetcode.com/problems/encode-and-decode-tinyurl/)

## Approach 1: HashMap with Auto-Increment ID

### Solution
csharp
```csharp
// Time Complexity: O(1) for both encode and decode
// Space Complexity: O(n) where n is the number of URLs encoded
using System.Collections.Generic;

public class Codec {
    private Dictionary<int, string> map = new Dictionary<int, string>();
    private int id = 0;

    // Encodes a URL to a shortened URL.
    public string Encode(string longUrl) {
        id++;
        map[id] = longUrl;
        return "http://tinyurl.com/" + id; // Return the shortened URL
    }

    // Decodes a shortened URL to its original URL.
    public string Decode(string shortUrl) {
        int key = int.Parse(shortUrl.Replace("http://tinyurl.com/", ""));
        return map[key]; // Retrieve the original URL
    }
}
```

## Approach 2: HashMap with Randomized Key

### Solution
csharp
```csharp
// Time Complexity: O(1) for both encode and decode
// Space Complexity: O(n) where n is the number of URLs encoded
using System;
using System.Collections.Generic;

public class Codec {
    private Dictionary<string, string> shortToLong = new Dictionary<string, string>();
    private Dictionary<string, string> longToShort = new Dictionary<string, string>();
    private readonly string characters = "abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789";
    private readonly Random random = new Random();

    // Generate a random 6-character string
    private string GetRandomKey() {
        char[] key = new char[6];
        for (int i = 0; i < 6; i++) {
            key[i] = characters[random.Next(characters.Length)];
        }
        return new string(key);
    }

    // Encodes a URL to a shortened URL.
    public string Encode(string longUrl) {
        if (longToShort.ContainsKey(longUrl)) {
            return "http://tinyurl.com/" + longToShort[longUrl];
        }
        string key;
        do {
            key = GetRandomKey();
        } while (shortToLong.ContainsKey(key)); // Ensure uniqueness of the key
        shortToLong[key] = longUrl;
        longToShort[longUrl] = key;
        return "http://tinyurl.com/" + key;
    }

    // Decodes a shortened URL to its original URL.
    public string Decode(string shortUrl) {
        string key = shortUrl.Replace("http://tinyurl.com/", "");
        return shortToLong[key];
    }
}
```

## Approach 3: Hashing with Fixed-Length Encoding

### Solution
csharp
```csharp
// Time Complexity: O(1) for both encode and decode
// Space Complexity: O(n) where n is the number of URLs encoded
using System;
using System.Collections.Generic;

public class Codec {
    private Dictionary<string, string> map = new Dictionary<string, string>();
    private readonly string characters = "abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789";
    private readonly int BASE = "abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789".Length;
    private int id = 0;

    // Generate a hash-like key for the URL
    private string ToBase62(int id) {
        List<char> encoded = new List<char>();
        while (id > 0) {
            encoded.Add(characters[id % BASE]);
            id /= BASE;
        }
        encoded.Reverse();
        return new string(encoded.ToArray());
    }

    private int ToBase10(string base62) {
        int id = 0;
        foreach (char c in base62) {
            id = id * BASE + characters.IndexOf(c);
        }
        return id;
    }

    // Encodes a URL to a shortened URL.
    public string Encode(string longUrl) {
        id++;
        string shortKey = ToBase62(id);
        map[shortKey] = longUrl;
        return "http://tinyurl.com/" + shortKey;
    }

    // Decodes a shortened URL to its original URL.
    public string Decode(string shortUrl) {
        string key = shortUrl.Replace("http://tinyurl.com/", "");
        return map[key];
    }
}
```

