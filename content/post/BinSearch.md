---
title: Binary Search - Simple, but Genius
subtitle: A simple yet quite efficient algorithm 
date: 2016-03-08
tags: ["example", "code"]
---

Binary search is an efficient algorithm for finding an item from a sorted list of items. It works by repeatedly dividing in half the portion of the list that could contain the item, until you've narrowed down the possible locations to just one. 

<!--more-->

Here is the psuedo-code:

{{< highlight javascript "linenos=inline">}}

    Let min = 1, max = 2
    Guess the average of maxmaxm, a, x and minminm, i, n, rounded down so that it is an integer.
    If you guessed the number, stop. You found it!
    If the guess was too low, set minminm, i, n to be one larger than the guess.
    If the guess was too high, set maxmaxm, a, x to be one smaller than the guess.
    Go back to step two.
{{</ highlight >}}

Here is the code in python:

```javascript
    def binary_search(arr, low, high, x):
        if high >= low:
            mid = (high + low) // 2
            if arr[mid] == x:
                return mid
            elif arr[mid] > x:
                return binary_search(arr, low, mid - 1, x)
            else:
                return binary_search(arr, mid + 1, high, x)
        else:
            return -1
    
    arr = [ 2, 3, 4, 10, 40 ]
    x = 10
    result = binary_search(arr, 0, len(arr)-1, x)
    if result != -1:
        print("Element is present at index", str(result))
    else:
        print("Element is not present in array")
```
