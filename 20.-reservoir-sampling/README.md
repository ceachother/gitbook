# 21.Reservoir Sampling

## Proof

### [http://blog.jobbole.com/42550/](http://blog.jobbole.com/42550/)

> 如前面所说，对这个问题我们首先从最简单的例子出发：数据流只有一个数据。我们接收数据，发现数据流结束了，直接返回该数据，该数据返回的概率为1。看来很简单，那么我们试试难一点的情况：假设数据流里有两个数据。
>
> 我们读到了第一个数据，这次我们不能直接返回该数据，因为数据流没有结束。我们继续读取第二个数据，发现数据流结束了。因此我们只要保证以相同的概率返回第一个或者第二个数据就可以满足题目要求。因此我们生成一个0到1的随机数R,如果R小于0.5我们就返回第一个数据，如果R大于0.5，返回第二个数据。
>
> 接着我们继续分析有三个数据的数据流的情况。为了方便，我们按顺序给流中的数据命名为1、2、3。我们陆续收到了数据1、2和前面的例子一样，我们只能保存一个数据，所以必须淘汰1和2中的一个。应该如何淘汰呢？不妨和上面例子一样，我们按照二分之一的概率淘汰一个，例如我们淘汰了2。继续读取流中的数据3，发现数据流结束了，我们知道在长度为3的数据流中，如果返回数据3的概率为1/3,那么才有可能保证选择的正确性。也就是说，目前我们手里有1,3两个数据，我们通过一次随机选择，以1/3的概率留下数据3，以2/3的概率留下数据1.那么数据1被最终留下的概率是多少呢？
>
> * 数据1被留下：$$(1/2)*(2/3) = 1/3$$ 
> * 数据2被留下概率：$$(1/2)*(2/3) = 1/3$$ 
> * 数据3被留下概率：$$1/3$$ 
>
> 这个方法可以满足题目要求，所有数据被留下返回的概率一样！
>
> 因此，我们做一下推论：假设当前正要读取第n个数据，则我们以1/n的概率留下该数据，否则留下前n-1个数据中的一个。以这种方法选择，所有数据流中数据被选择的概率一样。简短的证明：假设n-1时候成立，即前n-1个数据被返回的概率都是1/n-1,当前正在读取第n个数据，以1/n的概率返回它。那么前n-1个数据中数据被返回的概率为：$$(1/(n-1))*((n-1)/n)= 1/n$$ ，假设成立。
>
>  这就是所谓的蓄水池抽样算法。它在分析一些大数据集的时候非常有用。你可以在[这里](http://gregable.com/2007/10/reservoir-sampling.html)找到Greg写的关于蓄水池抽样的算法介绍。本文后面会介绍一下在Cloudera ML中使用的两种：分布式蓄水池抽样和加权分布式蓄水池抽样。

### Wikipedia

> Suppose we see a sequence of items, one at a time. We want to keep ten items in memory, and we want them to be selected at random from the sequence. If we know the total number of items \(n\), then the solution is easy: select ten distinct indices i between 1 and n with equal probability, and keep the $$i$$-th elements. The problem is that we do not always know n in advance. A possible solution is the following:
>
> * Keep the first ten items in memory.
> * When the $$i$$-th item arrives \(for $$i>10$$\):
>   * with probability $$10/i$$, keep the new item \(discard an old one, selecting which to replace at random, each with chance 1/10\)
>   * with probability $$1-10/i$$, keep the old items \(ignore the new one\)
>
> So:
>
> * when the $$k$$ -th item comes\( $$k>10$$ \), the $$k$$-th item is kept with probability $$p(k)=\frac{10}{k}$$, the old items\(as a whole\) is kept with probability  $$1-p(k)=1-\frac{10}{k}$$; for each of the old items stay\(assume $$p(k-1) = \frac{10}{k-1}$$ \), that is $$p(k-1)((1-p(k))+p(k)*\frac{9}{10})=\frac{10}{k-1}((1-\frac{10}{k})+\frac{10}{k}*\frac{9}{10})$$. 
> * when there are 10 items or fewer, each is kept with probability 1;
> * when there are 11 items, each of them is kept with probability 10/11; for the old items, that is \(1\)\(1/11 + \(10/11\)\(9/10\)\) = 1/11 + 9/11 = 10/11;
> * when there are 12 items, the twelfth item is kept with probability 10/12, and each of the previous 11 items are also kept with probability \(10/11\)\(2/12 + \(10/12\)\(9/10\)\) = \(10/11\)\(11/12\) = 10/12;
> * by induction, it is easy to prove that when there are n items, each item is kept with probability 10/n.

