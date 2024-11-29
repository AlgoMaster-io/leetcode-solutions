# [Encode and Decode TinyURL](https://leetcode.com/problems/encode-and-decode-tinyurl/)

## Approach 1: HashMap with Auto-Increment ID

### Solution
python
```python
# Time Complexity: O(1) for both encode and decode
# Space Complexity: O(n) where n is the number of URLs encoded

class Codec:
    def __init__(self):
        self.map = {}
        self.id = 0

    def encode(self, longUrl: str) -> str:
        self.id += 1
        self.map[self.id] = longUrl
        return f"http://tinyurl.com/{self.id}"

    def decode(self, shortUrl: str) -> str:
        key = int(shortUrl.replace("http://tinyurl.com/", ""))
        return self.map.get(key)
```

## Approach 2: HashMap with Randomized Key

### Solution
python
```python
# Time Complexity: O(1) for both encode and decode
# Space Complexity: O(n) where n is the number of URLs encoded
import random

class Codec:
    def __init__(self):
        self.shortToLong = {}
        self.longToShort = {}
        self.characters = "abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789"

    def getRandomKey(self) -> str:
        return ''.join(random.choice(self.characters) for _ in range(6))

    def encode(self, longUrl: str) -> str:
        if longUrl in self.longToShort:
            return "http://tinyurl.com/" + self.longToShort[longUrl]
        key = self.getRandomKey()
        while key in self.shortToLong:
            key = self.getRandomKey()
        self.shortToLong[key] = longUrl
        self.longToShort[longUrl] = key
        return "http://tinyurl.com/" + key

    def decode(self, shortUrl: str) -> str:
        key = shortUrl.replace("http://tinyurl.com/", "")
        return self.shortToLong.get(key)
```

## Approach 3: Hashing with Fixed-Length Encoding

### Solution
python
```python
# Time Complexity: O(1) for both encode and decode
# Space Complexity: O(n) where n is the number of URLs encoded

class Codec:
    def __init__(self):
        self.map = {}
        self.characters = "abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789"
        self.BASE = len(self.characters)
        self.id = 0

    def toBase62(self, id: int) -> str:
        encoded = []
        while id > 0:
            encoded.append(self.characters[id % self.BASE])
            id //= self.BASE
        return ''.join(reversed(encoded))

    def toBase10(self, base62: str) -> int:
        id = 0
        for c in base62:
            id = id * self.BASE + self.characters.index(c)
        return id

    def encode(self, longUrl: str) -> str:
        self.id += 1
        shortKey = self.toBase62(self.id)
        self.map[shortKey] = longUrl
        return "http://tinyurl.com/" + shortKey

    def decode(self, shortUrl: str) -> str:
        key = shortUrl.replace("http://tinyurl.com/", "")
        return self.map.get(key)
```

