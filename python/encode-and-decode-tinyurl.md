[Leetcode 535: Encode and Decode TinyURL](https://leetcode.com/problems/encode-and-decode-tinyurl/)

## Approaches
- [Approach 1: Hash Map with Randomly Generated Keys](#approach-1-hash-map-with-randomly-generated-keys)
- [Approach 2: Base62 Encoding](#approach-2-base62-encoding)

## Approach 1: Hash Map with Randomly Generated Keys
This approach stores the original long URLs in a dictionary using randomly generated short keys.

### Intuition:
- We use a dictionary to map our randomly generated short URL (key) back to the original long URL.
- To generate a short key, we randomly pick characters from a character set (typically contains digits, lowercase, and uppercase letters).
- For encoding, we check if the generated key is unique. If not, regenerate until we do.
- The encoded short URL is formed by appending the unique key to a base URL.
- For decoding, simply look up the key in the dictionary to retrieve the original URL.

### Steps:
1. Generate a random key of fixed length.
2. Check the existence of this key in the dictionary to ensure uniqueness.
3. Store the mapping of this key to the original URL once a unique key is generated.
4. When decoding, retrieve the original URL using the stored key.

### Code:
```python
import random
import string

class Codec:
    def __init__(self):
        self.url_dict = {}
        self.base_url = "http://tinyurl.com/"
        self.key_length = 6
        self.charset = string.ascii_letters + string.digits

    def _generate_key(self):
        # Generate a random key of fixed length using the charset
        return ''.join(random.choice(self.charset) for _ in range(self.key_length))

    def encode(self, longUrl: str) -> str:
        # Generate a unique key and store the mapping
        key = self._generate_key()
        while key in self.url_dict:
            key = self._generate_key()
        self.url_dict[key] = longUrl
        return self.base_url + key

    def decode(self, shortUrl: str) -> str:
        # Extract the key and return the corresponding original URL
        key = shortUrl.replace(self.base_url, "")
        return self.url_dict.get(key, "")

# Time Complexity: 
# Encoding: O(1) in the average case, O(n) for collision resolution.
# Decoding: O(1) as dictionary lookup is constant time.

# Space Complexity: O(n) where n is the number of unique URLs that have been encoded.
```

## Approach 2: Base62 Encoding
This approach assigns incremental IDs to URLs and encodes them using a Base62 system.

### Intuition:
- Assign each new long URL an incremental ID (like a primary key).
- Convert the ID to a Base62 number that acts as a short URL component.
- Use two dictionaries: one to map from ID to URL and another from URL to ID to check for duplicates.
- For decoding, reverse the Base62 conversion to retrieve the ID and subsequently the original URL.

### Steps:
1. Upon encoding, assign an incremental ID to each URL (hash-like).
2. Convert this ID to Base62 notation to create a shortened form.
3. Store mappings from ID to URL and URL to ID.
4. For decoding, reverse the Base62 transformation to retrieve the ID.

### Code:
```python
class Codec:
    def __init__(self):
        self.id_to_url = {}
        self.url_to_id = {}
        self.base_url = "http://tinyurl.com/"
        self.charset = string.ascii_letters + string.digits
        self.counter = 0

    def _encode_id(self, id):
        # Convert an integer id into a base62 string
        if id == 0:
            return self.charset[0]
        base62 = []
        while id:
            id, rem = divmod(id, 62)
            base62.append(self.charset[rem])
        return ''.join(reversed(base62))

    def _decode_id(self, base62_str):
        # Convert a base62 string back into an integer
        id = 0
        for char in base62_str:
            id = id * 62 + self.charset.index(char)
        return id

    def encode(self, longUrl: str) -> str:
        # Return the existing short URL if already encoded
        if longUrl in self.url_to_id:
            id = self.url_to_id[longUrl]
        else:
            # Allocate new ID and mappings
            self.counter += 1
            id = self.counter
            self.url_to_id[longUrl] = id
            self.id_to_url[id] = longUrl
        shortUrl = self.base_url + self._encode_id(id)
        return shortUrl

    def decode(self, shortUrl: str) -> str:
        base62_str = shortUrl.replace(self.base_url, "")
        id = self._decode_id(base62_str)
        return self.id_to_url.get(id, "")

# Time Complexity: 
# Encoding: O(log(n)) due to base conversion, where n is the number of URLs.
# Decoding: O(log(n)) for the reverse conversion.

# Space Complexity: O(n), where n is the number of unique URLs stored.
```

Both approaches ensure that URLs are encoded and decoded efficiently while maintaining uniqueness constraints for shortened URLs.

