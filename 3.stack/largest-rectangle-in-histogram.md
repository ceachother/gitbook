# 84. Largest Rectangle in Histogram

84. Largest Rectangle in Histogram

> Given _n_ non-negative integers representing the histogram's bar height where the width of each bar is 1, find the area of largest rectangle in the histogram.
>
> ![](https://leetcode.com/static/images/problemset/histogram.png)  
> Above is a histogram where width of each bar is 1, given height = `[2,1,5,6,2,3]`.
>
> ![](https://leetcode.com/static/images/problemset/histogram_area.png)  
> The largest rectangle is shown in the shaded area, which has area = `10` unit.
>
> **Example:**
>
> ```text
> Input: [2,1,5,6,2,3]
> Output: 10
> ```

## Solution 1:**Brute Force \(Time Limit Exceeded\)**

```text
public class Solution {
    public int largestRectangleArea(int[] heights) {
        int maxarea = 0;
        for (int i = 0; i < heights.length; i++) {
            int minheight = Integer.MAX_VALUE;
            for (int j = i; j < heights.length; j++) {
                minheight = Math.min(minheight, heights[j]);
                maxarea = Math.max(maxarea, minheight * (j - i + 1));
            }
        }
        return maxarea;
    }
}
```

### Complexity

* Time complexity : O\(n^2\). Every possible pair is considered
* Space complexity : O\(1\). No extra space is used.

## Solution 2: **Divide and Conquer Approach \(Time Limit Exceeded\)**

```text
public class Solution {
    public int calculateArea(int[] heights, int start, int end) {
        if (start > end)
            return 0;
        int minindex = start;
        for (int i = start; i <= end; i++)
            if (heights[minindex] > heights[i])
                minindex = i;
        return Math.max(heights[minindex] * (end - start + 1), Math.max(calculateArea(heights, start, minindex - 1), calculateArea(heights, minindex + 1, end)));
    }
    public int largestRectangleArea(int[] heights) {
        return calculateArea(heights, 0, heights.length - 1);
    }
}
```

**Complexity Analysis**

* Time complexity :

  Average Case: $$O\big(n \log(n)\big)$$.

  Worst Case: $$O(n^2)$$. If the numbers in the array are sorted, we don't gain the advantage of divide and conquer.

* Space complexity : $$O(n)$$. Recursion with worst case depth nn.

## Solution 3: **Stack\(AC\)**

思路：这题的一个基本思想是以每一个bar为最低点，向左右遍历直到遇到比他小的bar或边界。这样就能找到一个此bar为最低点的矩形面积。遍历所有的bar之后即可找到最大的矩形面积。但是向左右遍历寻找比他小的bar的时间复杂度是O\(n\)，在加上遍历一遍所有的bar，总的时间复杂度将为O\(n\*n\)，是无法通过所有数据的。因此我们需要寻找一种时间复杂度更低的寻找一个bar左右边界的算法。在网上流传了一个设计极其巧妙的方法，借助一个stack可以将时间复杂度降为O\(n\)。

这种算法的思想是维护一个递增的栈，这个栈保存了元素在数组中的位置。 这样在栈中每一个左边的bar都比本身小，所以左边就天然有界了，也就是左边界就是左边的一个bar（exclusive）。遍历一遍height数组，在将height数组入栈的时候，如果当前元素height\[i\]比栈顶元素小，则我们又找到了栈顶元素的右边界（exclusive）。因此我们在此时就可以计算以栈顶元素为最低bar的矩形面积了，因为左右边界我们都已经找到了，而且是在O\(1\)的时间复杂度内找到的。然后就可以将栈顶元素出栈了。这样每出栈一个元素，即计算以此元素为最低点的矩形面积。当最终栈空的时候我们就计算出了以所有bar为最低点的矩形面积。为保证让所有元素都出栈，我们在height数组最后加一个0，因为一个元素要出栈必须要遇到一个比他小的元素，也就是右边界。

作者：小榕流光 来源：CSDN 原文：[https://blog.csdn.net/qq508618087/article/details/50336795](https://blog.csdn.net/qq508618087/article/details/50336795) 版权声明：本文为博主原创文章，转载请附上博文链接！

The key point of stack solution is every new element added into the stack is greater than current top. 

If a new element is smaller than the top but greater than the next top\( say 4 come into \[1,3,5\]\), the max area for current top would be height\_of\_top \* \(right\_of\_rectangle - left\_of\_rectangle - 1\). e.g.:\[1,3,5\] + 4 -&gt; \[1,3,4\]

After 4 popping in, it becomes \[1,3,4\], then if another 2 came, the max area for element 4 is: 4 \* \(4 - 1 - 1\). the meaning of this is: 4 is the minimal height of the \(5,4\) rectangle, so the height of the max area is 4, the width is the right\_of\_rectangle\(4\) - left\_of\_rectangle\(1\)-1.

After 4 popping out, the max area for element 3 is: 3 \* \(4-1-0\). the meaning is: 3 is the minimal height of the \(3,5,4\) rectangle, so the height of max area is 4, the width is the right\_of\_rectangle\(4\) - left\_of\_rectangle\(1\)-1.

```java
public class Solution {
    public int largestRectangleArea(int[] heights) {
        Stack < Integer > stack = new Stack < > ();
        stack.push(-1);
        int maxarea = 0;
        for (int i = 0; i < heights.length; ++i) {
            while (stack.peek() != -1 && heights[stack.peek()] >= heights[i])
                maxarea = Math.max(maxarea, heights[stack.pop()] * (i - stack.peek() - 1));
            stack.push(i);
        }
        while (stack.peek() != -1)
            maxarea = Math.max(maxarea, heights[stack.pop()] * (heights.length - stack.peek() -1));
        return maxarea;
    }
}
```

```python
class Solution:
    def largestRectangleArea(self, heights):
        """
        :type heights: List[int]
        :rtype: int
        """
        heights.append(0)
        stack = collections.deque()
        stack.append(-1)
        ans = 0
        ## increasing stack
        for i in range(len(heights)):
            while heights[stack[-1]]>heights[i]:
                h = heights[stack.pop()]
                w = i-1 - stack[-1]
                ans = max(h*w, ans)
            stack.append(i)
        return ans
        
```

**Complexity Analysis**

* Time complexity : $$O(n)$$. nn numbers are pushed and popped.
* Space complexity : $$O(n)$$. Stack is used.

