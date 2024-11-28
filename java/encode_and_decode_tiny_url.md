# [Encode and Decode TinyURL](https://leetcode.com/problems/encode-and-decode-tinyurl/)

## Approach 1: HashMap with Auto-Increment ID

### Solution
```java
// Time Complexity: O(1) for both encode and decode
// Space Complexity: O(n) where n is the number of URLs encoded
import java.util.HashMap;

public class Codec {
    private HashMap<Integer, String> map = new HashMap<>();
    private int id = 0;

    // Encodes a URL to a shortened URL.
    public String encode(String longUrl) {
        id++;
        map.put(id, longUrl);
        return "http://tinyurl.com/" + id; // Return the shortened URL
    }

    // Decodes a shortened URL to its original URL.
    public String decode(String shortUrl) {
        int key = Integer.parseInt(shortUrl.replace("http://tinyurl.com/", ""));
        return map.get(key); // Retrieve the original URL
    }
}
```

## Approach 2: HashMap with Randomized Key

### Solution
```java
// Time Complexity: O(1) for both encode and decode
// Space Complexity: O(n) where n is the number of URLs encoded
import java.util.HashMap;

public class Codec {
    private HashMap<String, String> shortToLong = new HashMap<>();
    private HashMap<String, String> longToShort = new HashMap<>();
    private final String characters = "abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789";

    // Generate a random 6-character string
    private String getRandomKey() {
        StringBuilder key = new StringBuilder();
        for (int i = 0; i < 6; i++) {
            key.append(characters.charAt((int) (Math.random() * characters.length())));
        }
        return key.toString();
    }

    // Encodes a URL to a shortened URL.
    public String encode(String longUrl) {
        if (longToShort.containsKey(longUrl)) {
            return "http://tinyurl.com/" + longToShort.get(longUrl);
        }
        String key;
        do {
            key = getRandomKey();
        } while (shortToLong.containsKey(key)); // Ensure uniqueness of the key
        shortToLong.put(key, longUrl);
        longToShort.put(longUrl, key);
        return "http://tinyurl.com/" + key;
    }

    // Decodes a shortened URL to its original URL.
    public String decode(String shortUrl) {
        String key = shortUrl.replace("http://tinyurl.com/", "");
        return shortToLong.get(key);
    }
}
```

## Approach 3: Hashing with Fixed-Length Encoding

### Solution
```java
// Time Complexity: O(1) for both encode and decode
// Space Complexity: O(n) where n is the number of URLs encoded
import java.util.HashMap;

public class Codec {
    private HashMap<String, String> map = new HashMap<>();
    private final String characters = "abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789";
    private final int BASE = characters.length();

    // Generate a hash-like key for the URL
    private String toBase62(int id) {
        StringBuilder encoded = new StringBuilder();
        while (id > 0) {
            encoded.append(characters.charAt(id % BASE));
            id /= BASE;
        }
        return encoded.reverse().toString();
    }

    private int toBase10(String base62) {
        int id = 0;
        for (char c : base62.toCharArray()) {
            id = id * BASE + characters.indexOf(c);
        }
        return id;
    }

    private int id = 0;

    // Encodes a URL to a shortened URL.
    public String encode(String longUrl) {
        id++;
        String shortKey = toBase62(id);
        map.put(shortKey, longUrl);
        return "http://tinyurl.com/" + shortKey;
    }

    // Decodes a shortened URL to its original URL.
    public String decode(String shortUrl) {
        String key = shortUrl.replace("http://tinyurl.com/", "");
        return map.get(key);
    }
}
```