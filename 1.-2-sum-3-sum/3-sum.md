# 15. 3 sum

#### 15. 3Sum

> Given an array `nums` of _n_ integers, are there elements _a_, _b_, _c_ in `nums` such that _a_ + _b_ + _c_ = 0? Find all unique triplets in the array which gives the sum of zero.
>
> **Note:**
>
> The solution set must not contain duplicate triplets.
>
> **Example:**
>
> ```text
> Given array nums = [-1, 0, 1, 2, -1, -4],
>
> A solution set is:
> [
>   [-1, 0, 1],
>   [-1, -1, 2]
> ]
> ```

## Solution 1: DFS\(Time Limit Exceeded\)

The DFS solution have O\(n3\) time complexity which will get Time Limit Exceeded.

## Solution 2: Two Pointer\(AC\)

```text
public List<List<Integer>> threeSum(int[] nums) {
        int length = nums.length;
        Arrays.sort(nums);
        List<List<Integer>> result = new ArrayList<>();
        
        for(int i = 0; i<length-2; i++) {
            if(i>0 && nums[i] == nums[i-1]) {
                continue;
            }
            int first = nums[i];
            int left = i+1;
            int right = length-1;
            while(left < right) {
                int sum = nums[left] + nums[right];
                if(sum == 0-first) {
                    List<Integer> list = new ArrayList<>();
                    list.add(first);
                    list.add(nums[left]);
                    list.add(nums[right]);
                    result.add(list);
                    left++;
                    right--;
                } else if (sum > 0-first) {
                    right--;
                } else {
                    left++;
                }
            }
        }
        return result;
    }
```



