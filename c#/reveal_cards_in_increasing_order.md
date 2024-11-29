# [950. Reveal Cards In Increasing Order](https://leetcode.com/problems/reveal-cards-in-increasing-order/)

## Approach 1: Simulation Using Deque

### Solution
c#
// Time Complexity: O(n log n)
// Space Complexity: O(n)
using System;
using System.Collections.Generic;

public class Solution {
    public int[] DeckRevealedIncreasing(int[] deck) {
        int n = deck.Length;
        int[] result = new int[n];
        Array.Sort(deck); // Sort the deck in increasing order

        Deque<int> deque = new Deque<int>();

        // Add indices to the deque in order
        for (int i = 0; i < n; i++) {
            deque.AddLast(i);
        }

        // Place the smallest cards in the positions determined by deque
        foreach (int card in deck) {
            result[deque.RemoveFirst()] = card; // Place the current smallest card
            if (deque.Count > 0) {
                deque.AddLast(deque.RemoveFirst()); // Move the next index to the back
            }
        }

        return result;
    }
}

## Approach 2: Simulation Using Queue

### Solution
c#
// Time Complexity: O(n log n)
// Space Complexity: O(n)
using System;
using System.Collections.Generic;

public class Solution {
    public int[] DeckRevealedIncreasing(int[] deck) {
        Array.Sort(deck); // Sort the deck in increasing order
        Queue<int> queue = new Queue<int>();

        // Add indices to the queue in order
        for (int i = 0; i < deck.Length; i++) {
            queue.Enqueue(i);
        }

        int[] result = new int[deck.Length];

        // Place the smallest cards in the positions determined by the queue
        foreach (int card in deck) {
            result[queue.Dequeue()] = card; // Place the current smallest card
            if (queue.Count > 0) {
                queue.Enqueue(queue.Dequeue()); // Move the next index to the back
            }
        }

        return result;
    }
}

