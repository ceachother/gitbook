# Lintcode 645. Find the Celebrity

> Suppose you are at a party with `n` people \(labeled from `0` to `n - 1`\) and among them, there may exist one celebrity. The definition of a celebrity is that all the other `n - 1` people know him/her but he/she does not know any of them.
>
> Now you want to find out who the celebrity is or verify that there is not one. The only thing you are allowed to do is to ask questions like: "Hi, A. Do you know B?" to get information of whether A knows B. You need to find out the celebrity \(or verify there is not one\) by asking as few questions as possible \(in the asymptotic sense\).
>
> You are given a helper function `bool knows(a, b)` which tells you whether A knows B. Implement a function `int findCelebrity(n)`, your function should minimize the number of calls to `knows`.
>
> There will be exactly one celebrity if he/she is in the party. Return the celebrity's label if there is a celebrity in the party. If there is no celebrity, return `-1`.Have you met this question in a real interview?  YesProblem Correction
>
> #### Example
>
> Given n = `2`
>
> ```text
> 2 // next n * (n - 1) lines 
> 0 knows 1
> 1 does not know 0
> ```
>
> return `1` // 1 is celebrity

## Solution

```python
class Solution:
    # @param {int} n a party with n people
    # @return {int} the celebrity's label or -1
    def findCelebrity(self, n):
        ans = 0
        for i in range(0,n):
            if Celebrity.knows(ans, i):
                ans = i
        
        for j in range(0,n):
            if j==ans: continue
            if not Celebrity.knows(j, ans) or Celebrity.knows(ans, j): return -1
        return ans
```

