---
title: "Two Pointer Study 1"
publishDate: "1 August 2023"
description: "Two Pointer technique session 1, displaying three leetLeetCodecode solution with my thoughts"
tags: ["LeetCode", "two_pointer", "coding_technique"]
---

## Two Pointer Study - One

### LeetCode 167

LeetCode 167 is a simple variation of LeetCode 1, the "Two Sum" problem. If the array is sorted, there's no need to use a hash table. Instead, you can use two pointers converging towards the middle.

```python
def twoSum(self, numbers: List[int], target: int) -> List[int]:
        left, right = 0, len(numbers)-1
        while left < right:
            if numbers[left] + numbers[right] < target:
                left += 1
            elif numbers[left] + numbers[right] > target:
                right -= 1
            else:
                return [left+1, right+1]
        return [-1,-1]
```

### Leetcode 125

LeetCode 125 deals with palindromic strings. By establishing two pointers, one thing to note is how to handle situations where the character is null or non-alphanumeric. The solution is to update the two pointers separately: the left pointer increments while the right pointer decrements.

```python
def isPalindrome(self, s: str) -> bool:
        left, right = 0, len(s)-1
        while left <= right:
            while left < right and not s[left].isalnum():
                left += 1
            while left < right and not s[right].isalnum():
                right -= 1
            if s[left].lower() != s[right].lower():
                return False
            left += 1
            right -= 1
        return True
```

### Leetcode 3

LeetCode 3 is about the "Three Sum" problem. It builds on the logic of the "Two Sum". For an example array like [-5,-4,0,4,5], you'd start a for-loop, picking -5 first. Then, perform a "Two Sum" on the rest of the array [-4,0,4,5]. As you iterate, you can derive the solution. One thing to be cautious about is that duplicate results are not allowed in the answers. Hence, it's essential to sort the array first. After finding a solution, it's vital to check whether the neighbors in the direction of movement of the left and right pointers are duplicates. The left pointer moves to the right and the right pointer moves to the left. If duplicate data appears, you only need to adjust the pointers using a while loop.

```python
def threeSum(self, nums: List[int]) -> List[List[int]]:
        res = []
        nums.sort()
        for i in range(len(nums)):
            if nums[i] > 0:
                break
            if i == 0 or nums[i-1] != nums[i]:
                self.helper(nums, i, res)
        return res

    def helper(self, nums, i, res):
        left, right = i+1, len(nums)-1
        while left < right:
            sum = nums[i] + nums[left] + nums[right]
            if sum < 0:
                left += 1
            elif sum > 0:
                right -= 1
            else:
                res.append([nums[i], nums[left], nums[right]])
                left += 1
                right -= 1
                while left < right and nums[left] == nums[left-1]:
                    left += 1
                while left < right and nums[right] == nums[right+1]:
                    right -= 1
```
