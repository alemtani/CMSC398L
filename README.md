# CMSC398L

Using the Two Pointer technique to solve the LeetCode problem: [Container With Most Water](https://leetcode.com/problems/container-with-most-water/).

## Problem Description

![image](https://github.com/alemtani/CMSC398L/assets/37488089/f4d4329c-27e0-403b-b98b-46c7c673faf4)

![image](https://github.com/alemtani/CMSC398L/assets/37488089/3e0a5c07-43d2-4753-b02c-b1ac3fc691e4)

![image](https://github.com/alemtani/CMSC398L/assets/37488089/a8a36b60-3c65-4f00-ad23-c5f589ac61eb)

## Solution

### Approach 1: Brute Force

Simply iterate through every single pair in the array and find the pair with the maximum area.

> **Time Complexity:** `O(n^2)` since we are checking every pair.<br>
> **Space Complexity:** `O(1)` since we are not allocating any additional space.

### Approach 2: Two Pointers

Consider an arbitrary pair of indices `(i,j)` in the array (assume `i < j`). The area is constrained by the shorter height, so the area will be:

> `min(height[i], height[j]) * (j - i)`

Remember this fact when we walk through the following example.

![image](https://github.com/alemtani/CMSC398L/assets/37488089/3e0a5c07-43d2-4753-b02c-b1ac3fc691e4)

Let's start with `i` and `j` at the two ends of the array (i.e. `(i, j) = (0, 8)`). The shorter of the two heights is `height[i] = 1`, so the area is `1 * 8 = 8`. Now the big question: do we want to move `i` forward one, or `j` backward one? Consider that no matter what, we are decreasing the `j - i` term in the expression. Hence, it makes sense that we want to increase the `min(height[i], height[j])` term if we care about the maximum area. Since the area is constrained by `height[i]`, we want to move `i` forward by one in hopes of finding a taller height constraint.

OK, so now we have `(i, j) = (1, 8)`. The shorter of the two heights is `height[j] = 7`, so the area is `7 * 7 = 49`. Note that `49` is indeed the maximum area, but we have no way of knowing yet. We must continue until `i` and `j` intersect. This time, we move `j` down 1 since the area is constrained by `height[j]`.

Now we have `(i, j) = (1, 7)`. The shorter height is `height[j] = 3` and the area is `3 * 6 = 18`. Move `j` down by 1 again.

We have `(i, j) = (1, 6)`. This time we have `height[i] = height[j] = 8`, and the area is `8 * 5 = 40`. It does not matter which pointer we change, so let's just move `j` down 1 (to be consistent with the implementation later on).

We have `(i, j) = (1, 5)`. The constraint is `height[j] = 4` and the area is `4 * 4 = 16`. Move `j` down 1.

We have `(i, j) = (1, 4)`. The constraint is `height[j] = 5` and the area is `5 * 3 = 15`. Move `j` down 1.

We have `(i, j) = (1, 3)`. The constraint is `height[j] = 2` and the area is `2 * 2 = 4`. Move `j` down 1.

We have `(i, j) = (1, 2)`. The constraint is `height[j] = 6` and the area is `6 * 1 = 6`. Move `j` down 1.

Now we have `(i, j) = (1, 1)`. At this point, we have found our maximum area, which is `49`. Return that result.

> **Time Complexity:** `O(n)` since we use two pointers to do a single pass through the array.<br>
> **Space Complexity:** `O(1)` since we are not allocating any additional space.

## Implementation

```python
class Solution:
    def maxArea(self, height: List[int]) -> int:
        res = 0
        i, j = 0, len(height) - 1
        while i < j:
            res = max(res, min(height[i], height[j]) * (j - i))
            if height[i] < height[j]:
                i += 1
            else:
                j -= 1
        return res
```
