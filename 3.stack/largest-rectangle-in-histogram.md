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

* Time complexity : O\(n^2\)O\(n​2​​\). Every possible pair is considered
* Space complexity : O\(1\)O\(1\). No extra space is used.

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

  Average Case:O\big\(n \log\(n\)\big\)O\(nlog\(n\)\).

  Worst Case: O\(n^2\)O\(n​2​​\). If the numbers in the array are sorted, we don't gain the advantage of divide and conquer.

* Space complexity : O\(n\)O\(n\). Recursion with worst case depth nn.

## Solution 3: **Stack\(AC\)**

The key point of stack solution is every new element added into the stack is greater than current top. 

If a new element is smaller than the top but greater than the next top\( say 4 come into \[1,3,5\]\), the max area for current top would be height\_of\_top \* \(right\_of\_rectangle-left\_of\_rectangle-1\). e.g.:\[1,3,5\] + 4 -&gt; \[1,3,4\]

After 4 popping in, it becomes \[1,3,4\], then if another 2 came, the max area for element 4 is: 4 \* \(4 - 1 - 1\). the meaning of this is: 4 is the minimal height of the \(5,4\) rectangle, so the height of the max area is 4, the width is the right\_of\_rectangle\(4\) - left\_of\_rectangle\(1\)-1.

After 4 popping out, the max area for element 3 is: 3 \* \(4-1-0\). the meaning is: 3 is the minimal height of the \(3,5,4\) rectangle, so the height of max area is 4, the width is the right\_of\_rectangle\(4\) - left\_of\_rectangle\(1\)-1.



```text
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

**Complexity Analysis**

* Time complexity : O\(n\)O\(n\). nn numbers are pushed and popped.
* Space complexity : O\(n\)O\(n\). Stack is used.

