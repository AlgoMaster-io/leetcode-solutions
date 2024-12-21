Sure! Here are the solutions for the Encode and Decode TinyURL problem on LeetCode:

### LeetCode Problem: [Encode and Decode TinyURL](https://leetcode.com/problems/encode-and-decode-tinyurl/)

## Approaches

1. [Approach 1: HashMap with Simple Incremental ID](#approach-1)
2. [Approach 2: HashMap with Random and Base62 Encoding](#approach-2)

---

## Approach 1: HashMap with Simple Incremental ID

### Intuition:
The basic idea is to use a simple Integer counter to assign a unique ID to each URL. We maintain a mapping of the ID to the original URL using a HashMap. The ID is then converted to a string and appended to our base URL (e.g., http://tinyurl.com/).

### Steps:
1. Use a HashMap to store the mapping between an ID and the original URL.
2. Maintain an Integer counter that increments with each new URL.
3. To encode a URL, map it with the current counter value and increment the counter.
4. To decode, simply retrieve the URL from the HashMap using the ID.

### Time and Space Complexity:
- **Time**: O(1) for both `encode` and `decode` methods.
- **Space**: O(N), where N is the number of URLs encoded. We store each URL once.

```java
import java.util.HashMap;

public class Codec {
    private HashMap<Integer, String> map = new HashMap<>();
    private int id = 0;
    private static final String BASE_URL = "http://tinyurl.com/";

    public String encode(String longUrl) {
        // Store the long URL with current id
        map.put(id, longUrl);
        // Create a tiny URL by appending id to base URL and return it
        return BASE_URL + id++;
    }

    public String decode(String shortUrl) {
        // Extract the id from the short URL
        int id = Integer.parseInt(shortUrl.replace(BASE_URL, ""));
        // Get the long URL using the id from the map
        return map.get(id);
    }
}
```

---

## Approach 2: HashMap with Random and Base62 Encoding

### Intuition:
To improve collision handling and reduce the size of the keys, we use random strings as keys. Base62 encoding is employed for key generation, consisting of alphanumeric characters (a-z, A-Z, 0-9), allowing for a wide range of unique combinations.

### Steps:
1. Use a HashMap to map between encoded keys and URLs.
2. Generate a unique key for each URL using random strings.
3. Use Base62 encoding to ensure a consistent string length and avoid collisions.
4. To encode, generate a key, map it to the URL, and build the tiny URL.
5. To decode, retrieve the URL directly using the key.

### Time and Space Complexity:
- **Time**: O(1) for both `encode` and `decode` methods, on average.
- **Space**: O(N), where N is the number of URLs encoded. We store each URL once.

```java
import java.util.HashMap;
import java.util.Random;

public class Codec {

    private final String BASE_URL = "http://tinyurl.com/";
    private final HashMap<String, String> map = new HashMap<>();
    private final String characters = "abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789";
    private Random rand = new Random();
    private final int KEY_LENGTH = 6;
    
    public String encode(String longUrl) {
        String key;
        // Create a unique key of KEY_LENGTH randomly
        do {
            StringBuilder sb = new StringBuilder();
            for (int i = 0; i < KEY_LENGTH; i++) {
                sb.append(characters.charAt(rand.nextInt(characters.length())));
            }
            key = sb.toString();
        } while (map.containsKey(key)); // Ensure unique key by checking its existence

        // Map the key to the given url
        map.put(key, longUrl);
        // Return the tiny URL by appending key to base URL
        return BASE_URL + key;
    }

    public String decode(String shortUrl) {
        // Extract the key from the short URL
        String key = shortUrl.replace(BASE_URL, "");
        // Return the original URL mapped to the key
        return map.get(key);
    }
}
```

This approach ensures a balance between complexity and practicality with minimal chances of collisions while keeping the URL short and manageable.

