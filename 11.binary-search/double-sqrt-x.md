# double sqrt\(x\)

{% embed data="{\"url\":\"https://blog.csdn.net/xusiwei1236/article/details/25657611\",\"type\":\"link\",\"title\":\"【经典面试题】实现平方根函数sqrt - CSDN博客\",\"description\":\"本文描述了二分法、牛顿法、割线法的算法步骤，并实现了基于这几种方法的SQRT；同时，从理论角度解释了这些算法背后数学原理，并将这些方法推广到了求一般方程近似解的问题上。最后，对几种方法实现的sqrt的收敛速度进行了理论分析和实验对比。实验结果表明，牛顿法的收敛速度快于割线法，割线法快于二分法；与理论分析结果一致。\",\"icon\":{\"type\":\"icon\",\"url\":\"https://csdnimg.cn/public/favicon.ico\",\"aspectRatio\":0}}" %}

## Another solution: 

Because when x is smaller than 1, power\(x, 2\) &lt; x; when x is greater than 1, power\(x, 2\) &gt; x. So we can use Math.max\(1,x\) as the upper bound.

