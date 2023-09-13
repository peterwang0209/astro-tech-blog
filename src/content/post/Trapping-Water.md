---
title: "Two Pointer Study 2 - Trapping Water"
publishDate: "13 September 2023"
description: "Two Pointer technique session 2, displaying three leetcode solution with my thoughts, focusing on trapping water problem"
tags: ["leetcode", "two_pointer", "coding_technique"]
---

## Two Pointer Study - Two

### Leetcode 11

ChatGPT

Leetcode 11 lays the groundwork for the "trapping water" problem. To tackle it with the two pointers technique, understanding the conditions under which water gets trapped is essential. This demands a careful analysis of the problem statement. Here, we initiate two pointers, left and right. As we move the left pointer towards the right and vice versa, we simultaneously update the global_max. Given that we employ a while loop for iteration, determining the loop's termination conditions and the modifications to the `left` and `right` pointers is vital. In my approach, the `left` pointer moves if the height at its current position is less than the height at the right pointer. The idea is to seek a taller 'left' height. The converse is true for the `right` pointer.
A crucial code segment involves calculating the volume. Since the objective is to maximize water trapping, both width and height come into play. Here, height is determined by the lesser of the heights at the current `left` and `right` pointers, while width is simply the difference, `right - left`.

```python
def maxArea(self, height: List[int]) -> int:
        left, right = 0, len(height)-1
        local_max = 0
        global_max = 0
        while left < right:
            local_max = (right-left)*(min(height[left],height[right]))
            global_max = max(global_max, local_max)
            if height[left] < height[right]:
                left += 1
            else:
                right -= 1
        return global_max
```

### Leetcode 42

Leetcode 42 poses a classic "trapping rainwater" challenge. While both Leetcode 42 and 11 can employ a two-pointer strategy, they diverge in their objectives: Leetcode 42 aims to determine the total trapped water, whereas Leetcode 11 focuses on the maximum volume trapped between two pointers. Beyond the mere difference between the left and right pointers, it's also pivotal to contrast them with `left_most` and `right_most`.

To approach this, we initiate with `left_most`, left, `right_most`, and right. We employ a while loop to traverse the problem, setting the boundary condition as `left <= right`. This is because if the heights length is an odd number, merely using `left < right` might overlook the central case.

The crux of the problem lies in calculating trapped volume and discerning the conditions for its computation. If `left_most` is smaller than `right_most` and `height[left] < left_most`, the water that can be trapped is contingent upon the current `height[left]` and `left_most`. Relying on `right_most` here would cause a leak on the left side. However, if `height[left] > left_most`, there's no water to trap, necessitating an update of `left_most`.

Conversely, when `left_most >= right_most` and `height[right] < right_most`, the trapped water's volume is dictated by the `right_most`. Should `height[right] > right_most`, no water can be trapped, prompting an update to `right_most`.

```python
def trap(self, height: List[int]) -> int:
        if len(height) <= 2:
            return 0
        left_most, right_most = height[0], height[len(height)-1]
        left, right = 1, len(height)-2
        res = 0
        while left <= right:
            if left_most < right_most:
                if height[left] < left_most:
                    res += (left_most-height[left])
                else:
                    left_most = height[left]
                left += 1
            else:
                if height[right] < right_most:
                    res += (right_most-height[right])
                else:
                    right_most = height[right]
                right -= 1
        return res
```
