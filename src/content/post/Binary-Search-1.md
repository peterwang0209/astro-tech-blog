---
title: "Binary Search Study 1"
publishDate: "16 August 2023"
description: "Binary search study session 1, displaying three LeetCode solution with my thoughts, from the easy to difficult step by step"
tags: ["LeetCode", "binary_search", "two_pointer", "coding_technique"]
---

## Binary Search Study - One

### LeetCode 704

LeetCode 704 is the fundamental binary search question. It asks for the target location in a sorted ascending order. We can apply what we covered before, the two pointer technique. Define `left` and `right` pointer, using while loop to reduce the time complexity, notice setting the `left < right` or `left <= right` is a big deal, because this statement define when your while loop break. For my solution, I am using `left <= right`.
For `mid` calculation, `left + (right-left)//2` is considered as better approach as it prevents overloading. If we find the target, then we return the mid position, If the mid element is smaller than target, we should set the `left` to the `mid+1` because we know that the current `mid` is disqualified for solution.

```python
class Solution:
    def search(self, nums: List[int], target: int) -> int:
        left, right = 0, len(nums)-1
        while left <= right:
            mid = left + (right-left)//2
            if nums[mid] == target:
                return mid
            elif nums[mid] < target:
                left = mid + 1
            else:
                right = mid - 1
        return -1
```

### LeetCode 153

LeetCode asks for minimum in rotated sorted array. It takes one step further from the question above. When we rotate an array, it is easy to observe that the middle element is either greater or smaller than left or right element. From observing, we can tell that if the middle element is greater than right element, then not only the array has rotated, also the minimum element must exist between `mid+1` to `right`. Else it will exist between `left` to `mid`.
Why are we not using `right = mid - 1`?
Because in the previous question, we cover the case where m`id == target`, `mid < target`, and `mid > target`. In this example, we only consider `mid < target` and `mid >= target`. Thus we need to keep `right = mid` because we haven't check `nums[mid]` yet.
And when `left == right` it means we have found the smallest location, we should not proceed the while loop. So we break it.
We can return either `nums[left]` or `nums[right]`

```python
class Solution:
    def findMin(self, nums: List[int]) -> int:
        left, right = 0, len(nums)-1
        while left <= right:
            mid = left + (right-left)//2
            if nums[mid] > nums[right]:
                left = mid + 1
            else:
                right = mid
            if left == right:
                break
        return nums[right]
```

### LeetCode 33

This question takes another step further from the question we discussed above. Instead of finding the minimum value or the pivot point. We are looking to find a specific target. We can apply the basic binary search framework.
We are again checking if left part or right part are sorted and if the target element exist in those range.
There is a corner case, consider `[3,1]` and look for target `1`. `left = 0`, `right = 1`, `mid = 0 + (1-0)//2 = 0`. In this situation, the `left == mid`, if we change the `nums[left] <= nums[mid]` to `nums[left] < nums[mid]`, then we will never able to get the correct solution. because `nums[mid] = 3`, and 3 is not smaller than target, so we step into `right = mid - 1` which make `right = -1`.
When we are calculating `mid`, we typically round down the number, this means when you have only two elements remaining, mid will always be left.

```python
class Solution:
    def search(self, nums: List[int], target: int) -> int:
        left, right = 0, len(nums) - 1
        while left <= right:
            mid = left + (right - left) // 2
            if nums[mid] == target:
                return mid
            if nums[left] <= nums[mid]:
                if nums[left] <= target < nums[mid]:
                    right = mid - 1
                else:
                    left = mid + 1
            else:
                if nums[mid] < target <= nums[right]:
                    left = mid + 1
                else:
                    right = mid - 1
        return -1

```
