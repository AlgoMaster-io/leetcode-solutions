# [202. Happy Number](https://leetcode.com/problems/happy-number/)

## Approach 1: Using HashSet to Detect Cycles

### Solution
typescript
```typescript
// Time Complexity: O(log(n))
// Space Complexity: O(log(n))

function isHappy(n: number): boolean {
    const seen = new Set<number>();

    while (n !== 1 && !seen.has(n)) {
        seen.add(n);
        n = getNextNumber(n);
    }

    return n === 1;
}

function getNextNumber(n: number): number {
    let sum = 0;

    while (n > 0) {
        const digit = n % 10;
        sum += digit * digit;
        n = Math.floor(n / 10);
    }

    return sum;
}
```

## Approach 2: Fast and Slow Pointers (Optimal for Cycle Detection)

### Solution
typescript
```typescript
// Time Complexity: O(log(n))
// Space Complexity: O(1)

function isHappy(n: number): boolean {
    let slow = n;
    let fast = getNextNumber(n);

    while (fast !== 1 && slow !== fast) {
        slow = getNextNumber(slow);
        fast = getNextNumber(getNextNumber(fast));
    }

    return fast === 1;
}

function getNextNumber(n: number): number {
    let sum = 0;

    while (n > 0) {
        const digit = n % 10;
        sum += digit * digit;
        n = Math.floor(n / 10);
    }

    return sum;
}
```

