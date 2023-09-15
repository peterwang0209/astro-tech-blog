---
title: "Binary Search Study 2"
publishDate: "24 August 2023"
description: "Binary search study session 2, displaying three LeetCode solution with my thoughts, from the easy to difficult step by step"
tags: ["LeetCode", "binary_search", "two_pointer", "coding_technique"]
---

## Binary Search Study - Two

### LeetCode 74

LeetCode 74 asks us to implement a `O(log(m*n))` time complexity solution to search in a 2D array. My first thought was apply binary search 2 times, first to locate the row, and second to check if it is located inside that row. But I am quickly run into some corner cases. After reviewed some hints, I realize that we could process the row and col with assisting of total number mod and divide operation.
Assume total number is `T`, row is `r`, and col is `c`. `T = r*c`, if we want to know the location `X` where `0 <= X <= T`, we can use `X//col` to locate row and `X%col` to locate col

```python
class Solution:
    def searchMatrix(self, matrix: List[List[int]], target: int) -> bool:
        row = len(matrix)
        col = len(matrix[0])
        left, right= 0, row*col-1
        while left <= right:
            mid = left + (right-left)//2
            if matrix[mid//col][mid%col] == target:
                return True
            elif matrix[mid//col][mid%col] < target:
                left = mid + 1
            else:
                right = mid - 1
        return False
```

### LeetCode 875

LeetCode 875 teach me that binary search question can also ask you to process additional information instead of simple location question. This question ask how many banana koko needs to eat so that it will take exactly `h` days to finish.
One important things to notice is that how we set up the `left` and `right` pointer. Historically, we use `left` and `right` for array location index, but for this question we have an array but we use `left` and `right` to track how much banana can koko eat at one times. Thus `left = 1` and `right = max(piles)`
We are looking for minimum integer as solution, plus due to the nature of `//` round down the number, we set `must < h` and `must = h` together both lead to `right = mid - 1`

```python
class Solution:
    def minEatingSpeed(self, piles: List[int], h: int) -> int:
        left, right = 1, max(piles)
        while left <= right:
            must = 0
            mid = left + (right-left)//2
            for p in piles:
                must += math.ceil(p / mid)
            if must <= h:
                right = mid - 1
            else:
                left = mid + 1
        return left
```

### LeetCode 981 Time Based Key-Value Store

This question requires us to design a data structure. The idea is create a dictionary that has list as value using `defaultdict(list)`. Using `defaultdict` can save time because we do not need to check if key is exist or not.
When we set the value into the key, we pack `timestamp` and `value` together.
The `get` function contains the binary search algorithm. First we check if key is in dictionary or not. And then we handle the edge case where the smallest `timestamp` in the corresponding value is greater than `target timestamp` For the binary search algorithm, nothing special, when we find the matching one, we return the `lst[mid][1]`
Tricky part is what should we return if we did not find anything. Lets look at two example, `{foo:[[1:bar],[4:bar]]}`
if we try to find timestamp 2 which is between 1 and 4, then left and right start from 0 and 1, mid is 0. 1 is smaller than 2 thus we increment `left + 1`.
Now both left and right are equal, we do one further step, notice now mid is 1 because left and right are both 1, thus 4 is greater than 2. Right decrement by 1. Thus `right` is on the right location.
If we are finding 0, it is already being handled above as corner case, now only thing that have left is if the target timestamp is greater than the right, such as 5. In this scenario, when `left == right == 1`, the greatest number in the data structure is 4 which bring us to `left = mid + 1`. `left = 2, right = 1`.
After review the number, we still pick `right`

```python
class TimeMap:

    def __init__(self):
        self.cache = defaultdict(list)


    def set(self, key: str, value: str, timestamp: int) -> None:
        self.cache[key].append([timestamp, value])

    def get(self, key: str, timestamp: int) -> str:
        if key not in self.cache:
            return ""
        lst = self.cache[key]
        left, right = 0, len(lst)-1
        if lst[left][0] > timestamp:
            return ""
        while left <= right:
            mid = left + (right-left)//2
            if lst[mid][0] == timestamp:
                return lst[mid][1]
            elif lst[mid][0] < timestamp:
                left = mid+1
            else:
                right = mid-1
        return lst[right][1]
```
