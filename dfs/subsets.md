# 78. Subsets

> #### 78. Subsets
>
> Given a set of **distinct** integers, _nums_, return all possible subsets \(the power set\).
>
> **Note:** The solution set must not contain duplicate subsets.
>
> **Example:**
>
> ```text
> Input: nums = [1,2,3]
> Output:
> [
>   [3],
>   [1],
>   [2],
>   [1,2,3],
>   [1,3],
>   [2,3],
>   [1,2],
>   []
> ]
> ```

## Solution: DFS & Backtracking

```java
/**
  * DFS, backtracking
  */
class Solution {
    public List<List<Integer>> subsets(int[] nums) {
        List<List<Integer>> result = new ArrayList<>();
        List<Integer> list = new ArrayList<>();
        result.add(new ArrayList<>());
        dfs(nums, 0, list, result);
        return result;
    }
    
    public void dfs(int[] nums, int index, List<Integer> list, List<List<Integer>> result) {
        if(index >= nums.length) {
            return;
        }
        
        for(int i = index; i< nums.length; i++) {
            list.add(nums[i]);
            result.add(new ArrayList(list));
            dfs(nums, i+1, list, result);
            //reset
            list.remove(list.size()-1);
        }
    }
}
```

### Time Complexity

It has n iteration in the dfs for loop, for every iteration, it has f\(n-1\) + f\(n-2\) + ... + f\(1\) calling. Because of:

> ```text
> T(n) = T(n-1) + T(n-2) + ... + T(1) + 1
>      = T(n-1) + T(n-1)
>      = 2*T(n-1)
> T(n)/T(n-1) = 2
> T(n) = 2^(n-1)
> ```

so the time complexity would be O\(n \* 2^n\)

Another way to think is the array contains n numbers, each element will have two status: 0 and 1. 0 means it won't show up, 1 means it will. So it has in total 2^n combination\(eg. for {1,2,3}, it has 000,100,010,001,110,101,011,111\), for each combination, it will use O\(n\) to search.

